
### Deleting Comments 
Making a 'DELETE' request to server ./services/comments

```js
function deleteComment({postId, id}) {
return makeRequest(`posts/${postId}/comments/${id}`, {
	method: 'DELETE'
	});
};
```


### Toggling Comment Like

```js
export function toggleCommentLike({id, postId}) {
return makeRequest(`posts/${postId}/comments/${commentId}/toggleLike`, {method: 'POST'}
})
```


