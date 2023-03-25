<template lang="pug">
.elevator
  .elevator_scale
    .elevator_cabin(:style="{ bottom: cabinPosition + '%' }") 
      img(class="elevator_arrow" src="/icons/up.svg" alt="up" v-if="directionGlobal === 1 && isMoving")
      img(class="elevator_arrow" src="/icons/down.svg" alt="down" v-if="directionGlobal === -1 && isMoving")
    .elevator_floor(
      v-for="floor in 5"
      :key="floor"
      :style="{ bottom: (floor - 1) * 20 + '%' }"
    ) {{ floor }}
  .elevator_buttons
    button(
      v-for="floor in 5"
      :key="floor"
      @click="addToQueue(floor)"
      :class="{ active: queue.includes(floor) }"
      class="elevator_buttons_item"
    ) {{ floor }}
</template>

<script>

export default {
  data() {
    return {
      cabinPosition: 0,
      queue: [],
      isMoving: false,
      directionGlobal: 1,
    };
  },
  methods: {
    addToQueue(floor) {
      this.queue.push(floor);
      if (!this.isMoving) {
        this.moveCabin();
      }
    },
    moveCabin() {
      if (this.queue.length === 0) {
        this.isMoving = false;
        return;
      }
      this.isMoving = true;
      const nextPosition = (this.queue.shift() - 1) * 20;
      const distance = Math.abs(nextPosition - this.cabinPosition);
      const speed = 5000 / distance;
      const direction = nextPosition > this.cabinPosition ? 1 : -1;
      this.directionGlobal = direction;
      const interval = setInterval(() => {
        if (
          (direction === 1 && this.cabinPosition >= nextPosition) ||
          (direction === -1 && this.cabinPosition <= nextPosition)
        ) {
          clearInterval(interval);
          setTimeout(() => {
            this.moveCabin();
          }, 3000);
        } else {
          this.cabinPosition += direction;
        }
      }, speed);
    },
  }
};
</script>

<style scoped lang="scss">
.elevator {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 900px;
  background-image: url("/public/images/bg.jpg");
  background-position: center;
  background-size: contain;
  background-repeat: no-repeat;
  background-size: 580px 690px;
  background-position-y: 4rem;

  &_scale {
    position: relative;
    width: 40px;
    height: 400px;
    border: 1px solid black;
    margin-bottom: 20px;
    background-color: #fff;
    border: 1px solid transparent;
  }

  &_cabin {
    position: absolute;
    bottom: 0;
    width: 40px;
    height: 60px;
    background-color: rgb(197, 191, 191);
  }

  &_floor {
    position: absolute;
    bottom: -30px;
    width: 20px;
    height: 20px;
    line-height: 20px;
    text-align: center;
  }

  &_buttons {
    display: flex;
    justify-content: center;
    gap: 15px;

    &_item {
      padding: 15px 20px;
      font-size: 20px;
      border-radius: 50px;
      background-color: #60B8D1;
      border: none;
      cursor: pointer;

      &:hover {
        background-color: #279CC6;
        color: white;
      }
    }
  }

  &_arrow {
    width: 40px;
  }
}
.active {
  background-color: rgb(255, 255, 158);
}
</style>
