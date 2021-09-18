<template>
    <div class="columns is-centered">
        <div class="column is-8">
            <div class="card">
              <div class="card-content">
                <p> {{title}} ({{time}} horas) </p>
              </div>
              <footer class="card-footer">
                <a href="#" class="card-footer-item" @click.prevent="$emit('remove')">Delete</a>
              </footer>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'Task',
  emits: ['remove'],
  props: {
    title: String,
    time: Number
  }
}
</script>
