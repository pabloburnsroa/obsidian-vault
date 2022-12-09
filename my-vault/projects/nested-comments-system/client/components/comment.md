
Deleting a comment from a post 
```js
function onCommentDelete(){
return deleteCommentFn
	.execute({postId: post.id, id})
	.then(({data}) => deleteLocalComment(data.id))
}
```

Delete icon/button
```js
<IconBtn disabled={deleteCommentFn.loading} onClick={onCommentDelete} Icon={FaTrash} aria-lablel="Delete" color="danger"
```

Render error when attempting to delete 
```js
{deleteCommentFn.error && (
	<div>{deleteCommentFn.error}</div>
)}
```

Only owners of comments can edit/delete
This will retrieve userId from cookies, i.e. user "logged in"
```js
const currentUser = useUser()
```

```js
{user.id === currentUser.id &&
	// Render edit/delete buttons in comment component
	}
```


Get like count and likedByMe

```js
function onToggleCommentLike() {
	return toggleCommentLikeFn.execute({ id, postId: post.id}).then({addLike}) => toggleLocalCommentLike(id, addLike)

}
```

Like icon
```js
	<IconBtn
		onClick={onToggleCommentLike}
		disabled={toggleCommentLikeFn.loading}	
>
	{likeCount}
	</IconBtn>
```

