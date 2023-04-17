<template>
  <form>
    <div v-for="(flight, index) in flights" :key="index">
      <base-card>
        <div class="form-control">
          <label :for="'title' + index">Бортовой номер</label>
          <input
            :id="'title' + index"
            :name="'title' + index"
            type="text"
            :value="flight.title"
            @input="flight.title = $event.target.value"
          />
        </div>
        <div
          v-for="(segment, segmentIndex) in flight.segments"
          :key="segmentIndex"
        >
          <div class="form-control">
            <label :for="'departure' + index + '-' + segmentIndex">Вылет</label>
            <input
              :id="'departure' + index + '-' + segmentIndex"
              :name="'departure' + index + '-' + segmentIndex"
              type="datetime-local"
              :value="segment.departure"
              @input="segment.departure = $event.target.value"
            />
          </div>
          <div class="form-control">
            <label :for="'arrival' + index + '-' + segmentIndex">Посадка</label>
            <input
              :id="'arrival' + index + '-' + segmentIndex"
              :name="'arrival' + index + '-' + segmentIndex"
              type="datetime-local"
              :value="segment.arrival"
              @input="segment.arrival = $event.target.value"
            />
          </div>
        </div>
        <div class="form-control">
          <base-button type="button" @click="addAnotherSegment(index)">
            Добавить еще один перелет
          </base-button>
        </div>
      </base-card>
    </div>
  </form>
  <div class="button">
    <base-button type="button" @click="addAnotherFlight">
      Добавить воздушное судно
    </base-button>
    <button class="show" @click="drawSchedule">Показать</button>
  </div>

  <canvas ref="canvas" width="800" height="600"></canvas>
</template>

<script>
export default {
  data() {
    return {
      flights: [
        {
          title: "",
          segments: [
            {
              departure: "",
              arrival: "",
            },
          ],
        },
      ],
    };
  },
  methods: {
    addAnotherSegment(flightIndex) {
      this.flights[flightIndex].segments.push({
        departure: "",
        arrival: "",
      });
    },
    addAnotherFlight() {
      this.flights.push({
        title: "",
        segments: [
          {
            departure: "",
            arrival: "",
          },
        ],
      });
    },

    drawSchedule() {
      const canvas = document.querySelector("canvas");
      const ctx = canvas.getContext("2d");
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      let y = 50;
      let maxFlightHeight = 0;

      let maxTitleWidth = 0;
      this.flights.forEach((flight) => {
        ctx.font = "20px Arial";
        const titleWidth = ctx.measureText(flight.title).width;
        if (titleWidth > maxTitleWidth) {
          maxTitleWidth = titleWidth;
        }
      });

      this.flights.forEach((flight, index) => {
        const x = 50 + index;
        const width = 50;
        const height = 100;
        const padding = 10;

        flight.segments.forEach((segment) => {
          const departureDate = new Date(segment.departure);
          const arrivalDate = new Date(segment.arrival);

          const departureX =
            x +
            ((departureDate.getHours() * 60 + departureDate.getMinutes()) /
              1440) *
              (canvas.width - x - padding - width);
          const arrivalX =
            x +
            ((arrivalDate.getHours() * 60 + arrivalDate.getMinutes()) / 1440) *
              (canvas.width - x - padding - width);

          ctx.fillStyle = "blue";
          ctx.fillRect(departureX, y, arrivalX - departureX, height);
        });

        ctx.fillStyle = "black";
        ctx.font = "20px Arial";
        ctx.textAlign = "left";
        ctx.textBaseline = "top";
        ctx.fillText(
          flight.title,
          x + width / 2 - maxTitleWidth / 2,
          y - padding
        );

        maxFlightHeight = Math.max(maxFlightHeight, height);

        y += maxFlightHeight + padding;
      });
    },

    mounted() {
      this.drawSchedule();
    },
  },
};
</script>

<style>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
  margin: 0 10px 0 10px;
}

input {
  display: block;
  width: 30%;
  font: inherit;
  padding: 0.15rem;
  border: 1px solid #ccc;
}

input:focus {
  outline: none;
  border-color: #3a0061;
  background-color: #f7ebff;
}

.form-control {
  margin: 1rem 0;
  display: flex;
}
.button {
  position: relative;
  top: -20px;
  left: 162px;
}
.submit {
  position: relative;
  left: 10px;
}
.show {
  padding: 0.75rem 1.5rem;
  font-family: inherit;
  background-color: #610833;
  border: 1px solid #080808;
  border-radius: 10px;
  color: white;
  cursor: pointer;
}
.show:active,
.show:hover {
  background-color: #880c48;
}
</style>
