### delete comment

```js
server.delete('posts/:postId/comments/:commentId', async (req,res) => {
	const {userId} = await prisma.comment.findUnique({
		where: {id: req.params.commentId},
		select: {userId: true},
	});
// TODO error checking: check if user has permission to delete
	
	return await commitToDb(
		prisma.comment.delete({
		where: {id: req.params.commentId},
		select: {id: true},
		})
	)
})
```


### get comment

```js
server.get("/posts/:id", async (req,res) => {
	return await comitToDb(
		prisma.post.findUnique({
			where: {id: req.params.id},
			select: {
			body: true,
			title: true,
			comments: {
				orderBy: {
					createdAt: "desc"
				},
				select: {...COMMENT_SELECT_FIELDS,
				_count: { select: { likes: true} },
				},
				}
		}).then(
			async post => {
			const likes = await primsa.likes.findMany({
			where: {
			userId: req.cookies.userId,
			commentId: { in: post.comments.map(comment => comment.id)}
			}})
			return {
			...post, comments: post.comments.map(comment => {
			const { _count, ...commentFields} = comment 
			return {
				...commentFields,
				likeByMe: likes.find(like => like.commentId === comment.id), 
				likeCount: _count.likes
			}
			})
			}
			}
		))
})
```

### create comment

```ts
server.post(`/posts/:id/comments`, async ( req: FastifyRequest<{Body: {message: string; parentId: string; }; Params: { id: string; }; }>, res ) => {

if (req.body.message === '' || req.body.message == null) {
return res.send(server.httpErrors.badRequest('Message is required...'));
}

return await commitToDb( 
	prisma.comment.create(
		{ data: { message: req.body.message, userId: req.cookies.userId as any, parentId: req.body.parentId, postId: req.params.id, },
		select: COMMENT_SELECT_FIELDS, }).then (comment => {
			return {
			...comment,
			likeCount: 0,
			likeByMe: false
			}
		})
		
		); } );

```

### toggle comment like

```js
server.post(`/posts/:postId/comments/:commentId/toggleLike`, async (req,res) => {
	const data = { commentId: req.params.commentId, userId: req.cookies.userId}
	const like = await prisma.like.findUnique({ where: { userId_commentId: data}})
	if(like == null) {
		return await commitToDB(prisma.like.create({data})).then(() => {
		return { addLike: true} 
		})
	} else {
		return await commitToDB(prisma.like.delete({where: {userId_commentId: data }})).then(() => {
		return { addLike: false} 
		})
		
	}
})
```