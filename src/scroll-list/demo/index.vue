<template>
  <demo-section>
    <demo-block title="基础用法">
      <scroll-list style="height: 300px" :list="[1, 2, 3, 4, 5]">
        <template slot="default" slot-scope="scope">
          data: {{ scope.row }} isCur: {{ scope.isCur }}
        </template>
      </scroll-list>
    </demo-block>
    <demo-block title="鼠标进入暂停">
      <scroll-list style="height: 300px" :list="[1, 2, 3, 4, 5]" pauseOnMousein>
        <template slot="default" slot-scope="scope">
          data: {{ scope.row }}
        </template>
      </scroll-list>
    </demo-block>
  </demo-section>
</template>
