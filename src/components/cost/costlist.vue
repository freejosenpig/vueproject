<template>
	<!-- 分页 -->
	<div class="block">
	    <span class="demonstration">调整每页显示条数</span>
	    <el-pagination @size-change="handleSizeChange" background @current-change="handleCurrentChange"
	      v-model:currentPage="currentPage2" :page-sizes="[100, 200, 300, 400]" :page-size="100"
	      layout="sizes, prev, pager, next" :total="1000">
	    </el-pagination>
	  </div>
</template>

<script>
</script>

<style>
</style>
