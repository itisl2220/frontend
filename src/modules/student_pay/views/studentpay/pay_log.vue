<template>
	<cl-crud ref="Crud">
		<cl-row>
			<!-- 刷新按钮 -->
			<cl-refresh-btn />
			<!-- 新增按钮 -->
			<cl-add-btn />
			<!-- 删除按钮 -->
			<cl-multi-delete-btn />
			<cl-flex1 />
			<!-- 关键字搜索 -->
			<cl-search-key />
		</cl-row>

		<cl-row>
			<!-- 数据表格 -->
			<cl-table ref="Table" />
		</cl-row>

		<cl-row>
			<cl-flex1 />
			<!-- 分页控件 -->
			<cl-pagination />
		</cl-row>

		<!-- 新增、编辑 -->
		<cl-upsert ref="Upsert" />
	</cl-crud>
</template>

<script lang="ts" name="studentpay-pay_log" setup>
import { useCrud, useTable, useUpsert } from "@cool-vue/crud";
import { useCool } from "/@/cool";

const { service } = useCool();

// cl-upsert
const Upsert = useUpsert({
	items: [
		{
			label: "订单标题",
			prop: "title",
			component: { name: "el-input", props: { clearable: true } },
			required: true
		},
		{
			label: "订单号",
			prop: "orderId",
			hook: "number",
			component: { name: "el-input-number" },
			required: true
		},
		{
			label: "学生姓名",
			prop: "studentName",
			component: { name: "el-input", props: { clearable: true } },
			required: true
		},
		{
			label: "学生学号",
			prop: "studentNo",
			component: { name: "el-input", props: { clearable: true } },
			required: true
		},
		{
			label: "金额",
			prop: "amount",
			hook: "number",
			component: { name: "el-input-number" },
			required: true
		},
		{
			label: "状态",
			prop: "status",
			component: { name: "el-radio-group", options: [] },
			required: true
		},
		{
			label: "支付状态",
			prop: "payStatus",
			component: { name: "el-radio-group", options: [] },
			required: true
		},
		{
			label: "金流编号",
			prop: "newebPayNo",
			component: { name: "el-input", props: { clearable: true } }
		},
		{
			label: "支付方式",
			prop: "payType",
			component: { name: "el-radio-group", options: [] },
			required: true
		},
		{
			label: "完成时间",
			prop: "payTime",
			component: {
				name: "el-date-picker",
				props: { type: "date", valueFormat: "YYYY-MM-DD" }
			}
		},
		{
			label: "取号状态",
			prop: "takeNoStatus",
			component: { name: "el-radio-group", options: [] },
			required: true
		},
		{
			label: "备注",
			prop: "remark",
			component: { name: "el-input", props: { type: "textarea", rows: 4 } }
		}
	]
});

// cl-table
const Table = useTable({
	columns: [
		{ type: "selection" },
		{ label: "订单标题", prop: "title", minWidth: 140 },
		{ label: "订单号", prop: "orderId", minWidth: 140 },
		{ label: "学生姓名", prop: "studentName", minWidth: 140 },
		{ label: "学生学号", prop: "studentNo", minWidth: 140 },
		{ label: "金额", prop: "amount", minWidth: 140 },
		{ label: "状态", prop: "status", dict: [], dictColor: true, minWidth: 120 },
		{ label: "支付状态", prop: "payStatus", dict: [], dictColor: true, minWidth: 120 },
		{ label: "金流编号", prop: "newebPayNo", minWidth: 140 },
		{ label: "支付方式", prop: "payType", dict: [], dictColor: true, minWidth: 120 },
		{
			label: "完成时间",
			prop: "payTime",
			sortable: "custom",
			minWidth: 140,
			component: { name: "cl-date-text" }
		},
		{ label: "取号状态", prop: "takeNoStatus", dict: [], dictColor: true, minWidth: 120 },
		{ label: "备注", prop: "remark", showOverflowTooltip: true, minWidth: 200 },
		{
			label: "创建时间",
			prop: "createTime",
			minWidth: 160,
			component: { name: "cl-date-text" }
		},
		{
			label: "更新时间",
			prop: "updateTime",
			minWidth: 160,
			component: { name: "cl-date-text" }
		},
		{ type: "op", buttons: ["edit", "delete"] }
	]
});

// cl-crud
const Crud = useCrud(
	{
		service: service.studentpay.student_pay_order
	},
	(app) => {
		app.refresh();
	}
);
</script>
