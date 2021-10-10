<template>
  <Header :header="this.header" />
  <div class="content-container">
    <section class="section-container" id="missions" style="width:435px; height:714px;">
      <div class="section-header clipped-medium-backward">
        <img src="/icons/mission-icon.svg" />
        <h1>Mission Log</h1>
      </div>
      <div class="section-content-container">
        <h3>Current Assignment</h3>
        <Markdown :source="current_md" class="markdown" />
        <h3>Mission List</h3>
        <div class="mission-list-container">
          <Mission
            v-for="item in this.missions"
            :key="item.slug"
            :mission="item"
            :selected="this.mission_slug"
            @click="selectMission(item)"
          />
        </div>
      </div>
    </section>
    <section class="section-container" id="events" style="width:435px; height:714px;">
      <div class="section-header clipped-medium-backward">
        <img src="/icons/events-icon.svg" />
        <h1>Events Log</h1>
      </div>
      <div class="section-content-container">
        <Markdown :source="events" class="markdown" />
      </div>
    </section>
    <section class="section-container" id="pilots" style="width:894px; height:714px;">
      <div style="height:52px; overflow:hidden;">
        <div class="section-header clipped-medium-backward-pilot">
          <img src="/icons/pilot-icon.svg" />
          <h1>Pilot Roster</h1>
        </div>
        <div class="rhombus-back">&nbsp;</div>
      </div>
      <div class="section-content-container">
        <div class="pilot-list-container">
          <Pilot v-for="item in this.pilots" :key="item.slug" :pilot="item" />
        </div>
      </div>
    </section>
  </div>
  <svg
    style="visibility: hidden; position: absolute;"
    width="0"
    height="0"
    xmlns="http://www.w3.org/2000/svg"
    version="1.1"
  >
    <defs>
      <filter id="round">
        <feGaussianBlur in="SourceGraphic" stdDeviation="5" result="blur" />
        <feColorMatrix
          in="blur"
          mode="matrix"
          values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 19 -5"
          result="goo"
        />
        <feComposite in="SourceGraphic" in2="goo" operator="atop" />
      </filter>
    </defs>
  </svg>
  <audio autoplay>
    <source src="/startup.ogg" type="audio/ogg" />
  </audio>
  <Footer/>
</template>

<script>
import Header from './components/layout/Header.vue';
import Footer from './components/layout/Footer.vue';
import Mission from './components/Mission.vue';
import Pilot from './components/Pilot.vue';
import Markdown from 'vue3-markdown-it';

export default {
  components: {
    Header,
    Footer,
    Mission,
    Pilot,
    Markdown
  },

  data() {
    return {
      "mission_slug": "001",
      "current_md": "",
      "events": "",
      "missions": [
        {
          "slug": "001",
          "name": "Pitstop",
          "status": "start"
        },
       {
          "slug": "002",
          "name": "Operation Roanoke",
          "status": "start"
        },
      ],
      "pilots": [
        {
          "callsign": "PRESS",
          "alias": "Lysa Bench",
          "code": "6459512c-b008-42da-80d8-af02bdb5ecb9//7abb59c3-c14c-42a5-8bac-252736a35496",
          "corpro": "SSC",
          "frame": "Atlas",
          "mech": "BLOODY_SEVEN"
        },
        {
          "callsign": "RIFFRAFF",
          "alias": "Ridge Iscari",
          "code": "582ed8de-8422-4b5b-8a00-d0e6decbfa7c//6b940251-cc6e-4eec-990c-ec3e7a5b319d",
          "corpro": "SSC",
          "frame": "Swallowtail",
          "mech": "GARURU MK 16.7"
        },
        {
          "callsign": "RAVEN",
          "alias": "CWO JACE RAVEN",
          "code": "35b63169-1f4d-4bca-8e36-f58a1506aad5//5e327368-bf05-4bb0-996e-18441c4818fd",
          "corpro": "SSC",
          "frame": "Duskwing",
          "mech": "Wraith"
        },
        {
          "callsign": "STORMSHOT",
          "alias": "Latha Sarpy",
          "code": "2f07437b-adb5-4ff0-bc98-0b12a9ff0c29//f50adb54-8a1f-4201-8c16-22446bcb06fe",
          "corpro": "IPS-N",
          "frame": "Caliban",
          "mech": "A Little Shock"
        },
        {
          "callsign": "X",
          "alias": "Xerath Dinis VI",
          "code": "2f07437b-adb5-4ff0-bc98-0b12a9ff0c29//99b02e0f-0dc4-4e9e-b29d-7269d9e1c5ea",
          "corpro": "IPS-N",
          "frame": "Lancaster",
          "mech": "Merely Adrift"
        },
      ],
      "header": {
        "planet": "KIEMENE",
        "year": "5018u",
        "system": "BRIGHT",
        "gate": "WENSHAN",
        "ring": "ANNAMITE / DISTAL",
        "headerTitle": "UNION",
        "headerSubtitle": "DOJ/HR",
        "subheaderTitle": "LIBERATOR",
        "subheaderSubtitle": "DIAMOND DOGS",
      }
    }
  },

  created() {
    this.loadMissionMarkdown()
    this.loadEventsMarkdown()
  },

  computed: {

  },

  methods: {
    selectMission(mission) {
      this.mission_slug = mission.slug;
      this.loadMissionMarkdown()
    },
    loadMissionMarkdown() {
      let self = this;
      let md = `/missions/${self.mission_slug}.md`
      var client = new XMLHttpRequest();
      client.open('GET', md);
      client.onreadystatechange = function () {
        self.current_md = client.responseText;
      }
      client.send();
    },
    loadEventsMarkdown() {
      let self = this;
      let md = `/events.md`
      var client = new XMLHttpRequest();
      client.open('GET', md);
      client.onreadystatechange = function () {
        self.events = client.responseText;
      }
      client.send();
    }
  }

}
</script>


<style lang="scss">
#app {
  width: 1902px;
  height: 910px;
  overflow: hidden;
}
</style>
