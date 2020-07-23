<template>
  <div>
    <div class="ui two item menu">
      <a class="active item disabled"><b>Fancy Quiz App</b></a>
      <a class="item">Count:{{numCorrect}}/{{numTotal}}</a>
    </div>
  </div>
</template>


<script>
export default {
  props:[
    'numCorrect',
    'numTotal'
  ]
}
</script>