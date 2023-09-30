<template>
  <div>
    <div class="parent">
      <div class="child float-left"><img src="@/assets/logo.png" alt="Logo" class="component-logo" /></div>
      <div class="child float-right">
        <div class="timetable">
          <img v-if="weatherIcon" :src="'http://openweathermap.org/img/w/' + weatherIcon + '.png'" alt="Weather Icon"
            class="weather-icon">
          <div v-else>{{ weather ? weather : 'Loading...' }}</div>
          <span class="clock">{{ currentTime }}</span>
        </div>
        <div class="User logo">
          <el-button type="primary" icon="el-icon-user-solid" @click="toggleDropdown"></el-button>
          <div v-if="showDropdown" class="dropdown-menu">
            <a href="#" class="dropdown-item">
              <i class="el-icon-user"></i>
              Personal Profile
            </a>
            <a href="#" class="dropdown-item">
              <i class="el-icon-location"></i>
              Location
            </a>
            <a href="#" class="dropdown-item">
              <i class="el-icon-switch-button"></i>
              Sign Out
            </a>
            <!-- Add more options as necessary -->
          </div>
        </div>

      </div>
    </div>
    <div class="settings-container">
      <div class="sidebar">
        <button v-for="component in leftComponents" :key="component.name"
          :class="{ active: activeComponent === component.name }" @click="setActiveComponent(component.name)">
          {{ component.name }}
        </button>
      </div>
      <div class="content">
        <component v-for="component in filteredRightComponents" :key="component.name" :is="component.name" />
      </div>

    </div>
  </div>
</template>

<script>

import HomePage from './components/AppHomePage.vue';
import Calender from './components/AppCalender.vue';
import Friends from './components/AppFriends.vue';
import Setting from './components/AppSettings.vue';
import Location from './components/AppLocation.vue';
import Type from './components/AppType.vue';
import Notification from './components/AppNotification.vue';
import Privacy from './components/AppPrivacy.vue';
import Vue from 'vue';
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';

Vue.use(ElementUI);

export default {
  components: {
    HomePage,
    Calender,
    Friends,
    Setting,
    Location,
    Type,
    Notification,
    Privacy
  },
  data() {
    return {
      weatherIcon: null,
      showDropdown: false,
      weather: null,
      leftComponents: [
        { name: 'HomePage' },
        { name: 'Calender' },
        { name: 'Friends' },
        { name: 'Setting' }
      ],
      rightComponents: [
        { name: 'Location' },
        { name: 'Type' },
        { name: 'Notification' },
        { name: 'Privacy' }
      ],
      activeComponent: null, // <-- 在这里添加这一行
      currentTime: new Date().toLocaleTimeString()
    }
  },
  mounted() {
    this.interval = setInterval(() => {
      this.currentTime = new Date().toLocaleTimeString();
    }, 1000);
    this.fetchWeather();
  },
  beforeUnmount() {
    // your logic here
  },
  methods: {
    setActiveComponent(componentName) {
      if (componentName === 'HomePage') {
        this.activeComponent = 'Location';
      } if (componentName === 'Calender') {
        this.activeComponent = 'Type';
      } if (componentName === 'Friends') {
        this.activeComponent = 'Notification';
      } if (componentName === 'Setting') {
        this.activeComponent = 'Privacy';
      }
    },
    toggleDropdown() {
      this.showDropdown = !this.showDropdown;
    },
    async fetchWeather() {
      const apiKey = 'f2923985be31d9d2808a1c75979baa78';
      const location = 'Sydney,AU';
      try {
        const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${location}&appid=${apiKey}`);

        if (!response.ok) {
          throw new Error(`API responded with status: ${response.status}`);
        }

        const data = await response.json();
        if (data && data.weather && data.weather[0]) {
          this.weather = data.weather[0].description;  // Fetch the main weather description
          this.weatherIcon = data.weather[0].icon; // Fetch the icon code
        } else {
          console.error('Unexpected API response:', data);
        }
      } catch (error) {
        console.error('Failed to fetch weather:', error);
        this.weather = "Error fetching weather";
      }
    }
  },
  computed: {
    filteredRightComponents() {
      return this.rightComponents.filter(component => component.name === this.activeComponent);
    }
  }


}
</script>

<style scoped>
.component-logo {
  height: 100%;
  width: 100%;
  display: block;
  margin-right: 10px;
  /* Adjust as needed */
}

.settings-container {
  display: flex;
  height: 100vh;
}

.sidebar {
  flex: 2;
  overflow-y: auto;
}

.sidebar .active {
  background-color: black;
  color: white;
  /* Assuming you want the text to be white on a black background */
}


.content {
  flex: 8;
  overflow-y: auto;
}

.parent {
  display: flex;
  align-items: stretch;
}

.child {
  height: 200px;
  /* Assume this is the height you want */
  box-sizing: border-box;
  padding: 10px;
  border: 1px solid black;
}

.float-left {
  float: left;
  width: 20%;
}

.float-right {
  position: relative;
  float: right;
  width: 80%;
}

.timetable,
.User.logo {
  top: 0;
  height: 100%;
  width: 500px;
  /* ensures it takes the width of its content */
}

.timetable {
  top: 0;
  height: 100%;
  width: auto;
  right: 500px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  /* Add this for spacing between items */
}

.logo1,
.clock {
  flex: 1;
  /* 使两者都占据相同的空间 */
  display: flex;
  align-items: center;
  justify-content: center;
}

.logo1 {
  max-width: 100%;
  height: 100%;
}

.clock {
  font-size: 1.5em;
  /* 您可能需要调整这个大小，以使时间看起来与 logo 大小匹配 */
  margin-left: 10px;
  white-space: nowrap;
  /* 保持时间不换行 */
}

.dropdown-menu {
  position: absolute;
  top: 100%;
  /* Position it right below the logo */
  right: 0;
  /* Align it to the right side of the logo */
  width: 200px;
  /* Adjust as needed */
  background-color: white;
  border: 1px solid #ccc;
  box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2);
  z-index: 1;
}

.dropdown-item {
  display: flex;
  align-items: center;
  padding: 10px 20px;
  color: black;
  text-decoration: none;
  border-bottom: 1px solid #e0e0e0;
}


.dropdown-item:last-child {
  border-bottom: none;
}

.dropdown-item:hover {
  background-color: #f5f5f5;
}

button {
  padding: 10px 20px;
  margin: 5px 0;
  border: none;
  cursor: pointer;
  background-color: #f5f5f5;
  transition: background-color 0.3s;
}

button.active {
  background-color: black;
  color: white;
}

button:hover {
  background-color: #e0e0e0;
}
/* For the el-icon-user-solid icon */
.el-icon-user-solid {
  font-size: 500px;
}

/* For the clock */
.clock {
  font-size: 50px;
  margin-left: 10px;
  white-space: nowrap;
}

/* For the weather icon */
.weather-icon {
  height: 50px;
  width: 50px;
}
.timetable {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
/* Set the icon's font-size to fill the height of its container */
.float-right {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 80%;
}
.timetable,
.User.logo {
    height: 100%;
    width: 500px;
}


</style>
