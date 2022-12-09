
#### deleteLocalComment

```js
function deleteLocalComment(id) {
	setComments(prevComments => {
		return prevComments.filter(comment => comment.id !== id)
	})
}
```

### toggleLocalCommentLike

```js
function toggleLocalCommentLike(id, addLike) {
	setComments(prevComments => { 
		return prevComments.map(comment => {
			if(id === comment.id) {
				if(addLike) {	
					return {
						...comment,
						likeCount: comment.likeCount + 1,
						likedByMe: true
					}
				} else {
					return {
						...comment,
						likeCount: comment.likeCount - 1,
						likedByMe: false
					}
				} 
			} else {
					return comment
			}
		})})
}
```