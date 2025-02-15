<template>
  <b-container>
    <h1>{{ headerText }}</h1>
    <b-card-group deck>
      <b-card class="statsCards">
        <b-card-title class="title">
          {{ $t("page.landing.meeting.analytics.allCreatedSessionsTitle") }}
        </b-card-title>
        <div class="line"></div>
        <b-card-text>
          <h2 class="numbers">{{ tweenedOverAllSessions.toFixed(0) }}</h2>
          <div style="text-align: center">
            {{ allSessions }}
          </div>
          <br />
          <h2 class="numbers">{{ tweenedOverAllAttendees.toFixed(0) }}</h2>
          <div style="text-align: center">
            {{ allAttendees }}
          </div>
        </b-card-text>
      </b-card>
      <b-card class="statsCards">
        <b-card-title class="title">
          {{ $t("page.landing.meeting.analytics.lastMonthTitle") }}
        </b-card-title>
        <div class="line"></div>
        <b-card-text>
          <h2 class="numbers">
            {{ tweenedOverAllSessionsFromLastMonth.toFixed(0) }}
          </h2>
          <div style="text-align: center">
            {{ allSessions }}
          </div>
          <br />
          <h2 class="numbers">
            {{ tweenedOverAllAttendeesFromLastMonth.toFixed(0) }}
          </h2>
          <div style="text-align: center">
            {{ allAttendees }}
          </div>
        </b-card-text>
      </b-card>
      <b-card class="statsCards">
        <b-card-title class="title">
          {{ $t("page.landing.meeting.analytics.activeTitle") }}
        </b-card-title>
        <div class="line"></div>
        <b-card-text>
          <h2 class="numbers">
            {{ tweenedCurrentSessions.toFixed(0) }}
          </h2>
          <div style="text-align: center">
            {{ allSessions }}
          </div>
          <br />
          <h2 class="numbers">
            {{ tweendCurrentAttendees.toFixed(0) }}
          </h2>
          <div style="text-align: center">
            {{ allAttendees }}
          </div>
        </b-card-text>
      </b-card>
    </b-card-group>
    <br />
  </b-container>
</template>
<script lang="ts">
import Vue from "vue";
import gsap from "gsap";
import apiService from "@/services/api.service";
export default Vue.extend({
  name: "AnalyticsDataComponent",
  data() {
    return {
      loaded: false,
      overAllSessions: 0,
      overAllAttendees: 0,
      overAllSessionsFromLastMonth: 0,
      overAllAttendeesFromLastMonth: 0,
      currentSessions: 0,
      currentAttendees: 0,
      tweenedOverAllSessions: 0,
      tweenedOverAllAttendees: 0,
      tweenedOverAllSessionsFromLastMonth: 0,
      tweenedOverAllAttendeesFromLastMonth: 0,
      tweenedCurrentSessions: 0,
      tweendCurrentAttendees: 0,
    };
  },
  computed: {
    headerText() {
      return this.$t("page.landing.meeting.analytics.title");
    },
    allSessions() {
      return this.$t("page.landing.meeting.analytics.sessionText");
    },
    allAttendees() {
      return this.$t("page.landing.meeting.analytics.attendeesText");
    },
  },
  watch: {
    overAllSessions(n) {
      gsap.to(this, { duration: 1.25, tweenedOverAllSessions: Number(n) || 0 });
    },
    overAllAttendees(n) {
      gsap.to(this, { duration: 1.25, tweenedOverAllAttendees: Number(n) || 0 });
    },
    overAllSessionsFromLastMonth(n) {
      gsap.to(this, { duration: 1.25, tweenedOverAllSessionsFromLastMonth: Number(n) || 0 });
    },
    overAllAttendeesFromLastMonth(n) {
      gsap.to(this, {
        duration: 1.25,
        tweetweenedOverAllAttendeesFromLastMonthned: Number(n) || 0,
      });
    },
    currentSessions(n) {
      gsap.to(this, { duration: 1.25, tweenedCurrentSessions: Number(n) || 0 });
    },
    currentAttendees(n) {
      gsap.to(this, { duration: 1.25, tweendCurrentAttendees: Number(n) || 0 });
    },
  },
  created() {
    this.getAllDiveniData();
  },
  methods: {
    async getAllDiveniData() {
      this.loaded = false;
      let response = apiService.getAllDiveniData();
      let allData = await response.then(function (result) {
        let returnArray: Array<number> = [];
        returnArray.push(result.amountOfSessions);
        returnArray.push(result.amountOfAttendees);
        returnArray.push(result.amountOfSessionsLastMonth);
        returnArray.push(result.amountofAttendeesLastMonth);
        returnArray.push(result.amountOfSessionsCurrently);
        returnArray.push(result.amountOfAttendeesCurrently);
        return returnArray;
      });
      this.overAllSessions = allData[0];
      this.overAllAttendees = allData[1];
      this.overAllSessionsFromLastMonth = allData[2];
      this.overAllAttendeesFromLastMonth = allData[3];
      this.currentSessions = allData[4];
      this.currentAttendees = allData[5];
      this.loaded = true;
    },
  },
});
</script>
<style scoped>
.statsCards {
  text-align: center;
  border-radius: 2rem;
  background-color: var(--landingPageCardsBackground);
}

.line {
  background-color: rgba(0, 0, 0, 0.125);
  height: 1px;
  margin: 10px -20px;
}

.title {
  height: 60px;
  align-items: center;
  justify-content: center;
  display: flex;
}

@media (min-width: 768px) {
  .title {
    height: auto;
  }
}

@media (max-width: 575px) {
  .title {
    height: auto;
  }
}
</style>
