<template>
  <div class="Starter">
    <Ticket />
  </div>
</template>

<script>
import Ticket from './Starter/Ticket'
export default {
  components: {
    Ticket
  }
}
</script>

<style scoped>
.Starter {
  width: 100%;
  background: url('../assets/images/background.png');
  background-position: center;
  background-repeat: repeat no-repeat;
  background-size: contain;
  position: relative;
  height: 30em;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}
</style>
