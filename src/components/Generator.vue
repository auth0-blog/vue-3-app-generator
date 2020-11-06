<template>
<div class="generator">
  <section class="hero is-primary is-bold has-text-centered py-6">
    <div class="hero-body">
      <div class="container">
        <h1 class="title">
        What skills do you want to work on?
        </h1>
        <div v-for="skill in skillList" :key="skill.id">
          <div class="field">
            <label class="checkbox">
              <input type="checkbox" v-model="selectedSkills" :value="skill.id" @change="generateFilteredAppList">
              {{ skill.skill }}
            </label>
          </div>
        </div>
      </div>
    </div>
  </section>
      
  <div class="container">
    <div class="columns is-multiline mt-3">
      <div v-for="app in filteredAppList" :key="app.id" class="column is-one-third">
        <div class="card">
          <header class="card-header">
            <p class="card-header-title is-uppercase is-size-5">
              {{ app.app }}
            </p>  
          </header>
          <div class="card-content">
            <div class="content has-text-left"> 
              <p class="is-size-7 mb-4">{{ app.instructions }}</p>   
              <h4>Skills:</h4>
              <ul v-for="skill in app.skills" :key="skill.id">
                <li>
                  <strong>{{ skillList[skill-1].skill }}</strong>
                  <p class="skill-helper" v-if="skillList[skill-1].options" :set="randSkill = getRand(skillList[skill-1].options)">🦮 <a :href="randSkill">{{ randSkill }}</a></p>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import { ref } from 'vue';

export default {
  name: 'Generator',
  setup() {
    const GENERATOR_BASE = 'http://localhost:3000';
    const skillList = ref([]);
    const selectedSkills = ref([]);
    const filteredAppList = ref([]);
    let appList = [];

    // Update filtered application list based on skills selected
    function generateFilteredAppList() {
      filteredAppList.value = [];
      for (let i = 0; i < appList.length; i++) {
        if (checkArrIncludesEvery(appList[i].skills, selectedSkills.value) == true)
          filteredAppList.value.push(appList[i]);
      }
    }

    // Check if a given array contains every element in another array.
    // Returns boolean
    function checkArrIncludesEvery(comp, target) {
      return target.every(f => comp.includes(f));
    }

    // Get a random item from an array
    function getRand(value) {
      var keys = Object.keys(value);
      return value[keys[keys.length * Math.random() << 0]];
    }

    // Get the list of skills from the Express API
    async function getSkillList() {
      const response = await fetch(`${GENERATOR_BASE}/skills`);
      skillList.value = await response.json();
    }

    // Get the list of applications from the Express API
    async function getAppList() {
      const response = await fetch(`${GENERATOR_BASE}/apps`);
      appList = await response.json();
      filteredAppList.value = appList;
    }

    getSkillList();
    getAppList();

    return {
      skillList,
      selectedSkills,
      filteredAppList,
      generateFilteredAppList,
      getRand
    }
  }
}
</script>

<style scoped>
  .card {
    height: 100%;
  }
  label {
    font-size: 20px;
  }
  a {
    word-break: break-word;
  }
</style>
