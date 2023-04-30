<template>
  <div class="container">
    <AgreementList/>
    <NewTasksList/>
    <InProgressList/>
    <ReadyTasksList/>
    <FinalizeTasksList/>
  </div>
  <notifications position="bottom right" width="400px" classes="my-notification">
    <div class="notify__circle">dsadsada</div>
  </notifications>
</template>

<script>
import AgreementList from './components/AgreementList.vue';
import NewTasksList from './components/NewTasksList.vue';
import InProgressList from './components/InProgressList.vue';
import ReadyTasksList from './components/ReadyTasksList.vue';
import FinalizeTasksList from './components/FinalizeTasksList.vue'

export default {
    components: {
    AgreementList,
    NewTasksList,
    InProgressList,
    ReadyTasksList,
    FinalizeTasksList,
},
}
</script>

<style lang="scss">
* {
  font-family: 'Inter', sans-serif;
}

body {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.container {
  display: flex;
  flex-direction: row;
  align-items: flex-start;

  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 8px;

}
.my-notification {
  box-sizing: border-box;


  display: flex;
  flex-direction: row;
  align-items: flex-start;
  padding: 0px;

  min-height: 76px;

  word-wrap: break-word;

  background: #FFFFFF;

  border: 1px solid #E3E5E8;
  margin: 0 24px 24px 0;

  box-shadow: 0px 8px 8px rgba(0, 0, 0, 0.08), 0px 8px 16px rgba(0, 0, 0, 0.06);
  border-radius: 4px;
}

.notify__circle {
  width: 50px;
  height: 50px;
  background: #22C33D;
}

.notification-title {

  font-style: normal;
  font-weight: 584;
  font-size: 14px;

  word-wrap: break-word;

  margin-left: 64px;
  margin-top: 16px;

  color: #1C2530;
}
.notification-content {


  font-style: normal;
  font-size: 14px;


  word-wrap: break-word;
  margin: 8px 15px 16px 64px;


  color: #1C2530;
}

.vue-notification-template::before {
  content: '';
  position: absolute;
  width: 8px;
  height: 76px;
  background-color: #22C33D;
  border-radius: 4px 0px 0px 4px;

}

</style>
