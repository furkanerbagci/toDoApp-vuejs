<template>
    <div class="row">
      <div class="col-md-8 offset-md-2 d-flex justify-content-between border-bottom align-items-center py-3">
        <p >{{todo.text}}</p>
        <button class="btn btn-danger" @click="$emit(`deleteTodo`,todo.id)">Delete</button>
      </div>
    </div>
</template>

<script>
export default {
  props:["todo"]
}
</script>


