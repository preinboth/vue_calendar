<template>
  <div class="card" :class="cardClasses">
    <div
      class="card-header text-center"
      :class="cardHeaderClasses"
      role="button"
      @click="setActiveDay()"
    >
      <strong>{{ day.fullName }}</strong>
    </div>
    <div class="card-body">

      <transition name="fade" mode="out-in">

      <div v-if="day.events.length">
        <CalendarEvent
          v-for="event in day.events"
          :key="event.title"
          :event="event"
          :day="day"
        >
          <!-- <template v-slot:eventPriority="slotProbs"> -->
          <template #:eventPriority="slotProbs">
            <h6>{{ slotProbs.priorityDisplayName }}</h6>
          </template>

          <!-- <template v-slot="{ event }"> -->
          <template #default="{ event }">
            <i>{{ event.title }}</i>
          </template>
        </CalendarEvent>
      </div>

      <div v-else>
        <div class="alert alert-light text-center">
          <i>keine Termine</i>
        </div>
      </div>
    </transition>

    </div>
  </div>
</template>

<script>
import CalendarEvent from "./CalendarEvent";
import Store from "../store";

export default {
  name: "CalendarDay",
  components: {
    CalendarEvent,
  },
  // Array-Schreibweise --> nicht zu empfehlen
  // props: ["day"],

  //  Objekt Schreibweise
  props: {
    day: {
      type: Object,
      required: true,
      default: function () {
        return {
          id: -1,
          fullName: "fehlender Wochentag",
          events: [],
        };
      },
      validator: function (value) {
        if (Object.keys(value).includes("id")) {
          return true;
        }
      },
    },
    computed: {
      cardClasses() {
        return this.day.id === Store.getters.activeDay().id
          ? ["border-primary"]
          : null;
      },
      cardHeaderClasses() {
        return this.day.id === Store.getters.activeDay().id
          ? ["bg-primary", "text-white"]
          : null;
      },
    },
  },
  methods: {
    setActiveDay() {
      Store.mutations.setActiveDay(this.day.id);
    },
  },
};
</script>

<style scoped></style>
