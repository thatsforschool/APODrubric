<template>
    <section>
       
        <h1> {{title}}</h1>
        <p>{{author}}</p>
    </section>
</template>

<script>
export default {
  name: 'APODmain',
  props: {
    title: String,
    author: String
  }
}
</script>
<style></style>