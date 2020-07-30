<template>
  <div class="ticket-container">
    <div class="Ticket">
      <div class="stub-1">
        <div class="content">
          <p>{{ day }} {{ month }}</p>
          <span>{{ year }}</span>
          <!-- <img src="../../assets/images/barcode.png" /> -->
        </div>
      </div>
      <div class="stub-2">
        <div class="content">
          <p>Welcome to</p>
          <span>GMapsLocale</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      day: '01',
      month: 'JAN',
      year: '1999'
    }
  },
  mounted() {
    const dateTimeFormat = new Intl.DateTimeFormat('en', {
      year: 'numeric',
      month: 'short',
      day: '2-digit'
    })
    const date = dateTimeFormat.formatToParts(new Date())
    this.month = date[0].value.toUpperCase()
    this.day = date[2].value
    this.year = date[4].value
  }
}
</script>

<style scoped>
.ticket-container {
  position: relative;
}

.Ticket {
  display: flex;
  flex-direction: row;
  justify-content: center;
  font-family: 'Barlow Condensed', sans-serif;
  color: #7a7a7a;
  --dimension-w: calc(100px + (180 - 100) * ((100vw - 300px) / (1600 - 300)));
  --dimension-h: calc(var(--dimension-w) + 2em);
  height: var(--dimension-h);
  animation: swing-in-top-fwd 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275) both;
  animation-delay: 1s;
}

@keyframes swing-in-top-fwd {
  0% {
    transform: rotateX(-100deg);
    transform-origin: top;
    opacity: 0;
  }
  100% {
    transform: rotateX(0deg);
    transform-origin: top;
    opacity: 1;
  }
}

.stub-1 {
  display: flex;
  flex-direction: column;
  width: 28em;
  width: var(--dimension-w);
  height: 30em;
  height: var(--dimension-h);
  background-color: #fff;
  mask-image: url('../../assets/masks/stub1.svg');
  background-image: url('../../assets/images/barcode.png');
  background-repeat: no-repeat;
  background-position: left 20% top 50%;
  mask-position: center;
  background-size: 30%;
  mask-size: contain;
  mask-repeat: no-repeat;
  position: relative;
  border-right: 2px dashed #aaa;
}

.stub-1 > .content {
  margin: 1.5em 0;
  position: absolute;
  transform: rotate(90deg);
  top: 10%;
  right: 5%;
  display: flex;
  flex-direction: column;
}

.stub-1 > .content > p {
  font-size: calc(10px + (30 - 10) * ((100vw - 300px) / (1600 - 300)));
  text-rendering: optimizeLegibility;
}

.stub-1 > .content > span {
  font-size: calc(20px + (40 - 20) * ((100vw - 300px) / (1600 - 300)));
  line-height: calc(20px + (35 - 20) * ((100vw - 300px) / (1600 - 300)));
  text-rendering: optimizeLegibility;
}

.stub-1 > .content > img {
  margin-top: 2em;
  border: none;
  height: 10em;
}

.stub-2 {
  /* width: 62.4em; */
  width: calc(var(--dimension-w) * 2 + 2.4vw);
  /* height: 30em; */
  height: var(--dimension-h);
  background-color: #fff;
  mask-image: url('../../assets/masks/stub2.svg');
  mask-repeat: no-repeat;
  mask-position: center;
  mask-size: contain;
  background-image: url('../../assets/images/gmaps-logo.svg');
  background-position: bottom 0.5rem right -1rem;
  background-size: 40%;
  background-repeat: no-repeat;
}
@media only screen and (max-width: 600px) {
  .stub-2 {
    width: calc(var(--dimension-w) * 2 + 5vw);
  }
}
@media only screen and (max-width: 900px) {
  .stub-2 {
    width: calc(var(--dimension-w) * 2 + 3.5vw);
  }
}

.stub-2 > .content {
  margin: calc(20px + (50 - 20) * ((100vw - 300px) / (1600 - 300)));
  display: flex;
  flex-direction: column;
}

.stub-2 > .content > p {
  font-size: calc(10px + (25 - 10) * ((100vw - 300px) / (1600 - 300)));
  font-family: 'Overpass', sans-serif;
}

.stub-2 > .content > span {
  font-size: calc(20px + (40 - 20) * ((100vw - 300px) / (1600 - 300)));
  color: #4d4d4d;
  line-height: 2rem;
}
</style>
