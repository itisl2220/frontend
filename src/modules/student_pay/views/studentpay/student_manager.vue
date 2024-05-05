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

<script lang="ts" name="studentpay-student_manager" setup>
import { useCrud, useTable, useUpsert } from "@cool-vue/crud";
import { useCool } from "/@/cool";

const { service } = useCool();

// cl-upsert
const Upsert = useUpsert({
	items: [
		{
			label: "学生姓名",
			prop: "studentName",
			component: { name: "el-input", props: { clearable: true } },
			required: true,
			col: {
				span: 12
			}
		},
		{
			label: "学生性别",
			prop: "studentGender",
			component: {
				name: "el-radio-group",
				options: [
					{ label: "男", value: "男" },
					{ label: "女", value: "女" }
				],
				props: { clearable: true }
			},
			value: "男",
			col: {
				span: 12
			}
		},
		{
			label: "学生编号",
			prop: "studentId",
			component: { name: "el-input" },
			required: true,
			col: {
				span: 12
			}
		},
		{
			label: "学校编号",
			prop: "schoolId",
			component: { name: "el-input" },
			col: {
				span: 12
			}
		},
		{
			label: "学生身份证号",
			prop: "studentIdCard",
			component: { name: "el-input", props: { clearable: true } },
			col: {
				span: 24
			}
		},
		{
			label: "在学状态",
			prop: "studentStatus",
			// 单选框
			component: {
				name: "el-radio-group",
				options: [
					{ label: "在读", value: "1" },
					{ label: "休学", value: "2" },
					{ label: "退学", value: "3" },
					{ label: "毕业", value: "4" }
				],
				props: {}
			},
			value: "1",
			col: {
				span: 24
			}
		},
		{
			label: "户籍地址",
			prop: "registeredResidenceAddress",
			component: { name: "el-input" }
		},
		{
			label: "通信地址",
			prop: "studentAddress",
			component: { name: "el-input" }
		},
		{
			label: "同住家人",
			prop: "familyMembers",
			component: {
				name: "el-checkbox-group",
				options: [
					{ label: "父", value: "父" },
					{ label: "母", value: "母" },
					{ label: "祖父", value: "祖父" },
					{ label: "祖母", value: "祖母" },
					{ label: "外祖父", value: "外祖父" },
					{ label: "外祖母", value: "外祖母" },
					{ label: "兄弟姊妹", value: "兄弟姊妹" },
					{ label: "其他", value: "其他" }
				]
			},
			hook: {
				bind: (value, { form }) => {
					return value.split("|").filter(Boolean); // 结果为：[1, 2, 3]
				},
				submit: (value, { form }) => {
					return value.join("|");
				}
			}
		},
		{
			label: "特殊病史",
			prop: "specialMedicalHistory",
			// 该组件是多选框, 还有一个其他的input框
			component: {
				name: "el-checkbox-group",
				options: [
					{ label: "高血压", value: "高血压" },
					{ label: "癫痫", value: "癫痫" },
					{ label: "气喘", value: "气喘" },
					{ label: "糖尿病", value: "糖尿病" },
					{ label: "心脏病", value: "心脏病" },
					{ label: "異味性皮膚炎", value: "異味性皮膚炎" },
					{ label: "蠶豆症", value: "蠶豆症" },
					{ label: "其他", value: "其他" }
				],
				props: {}
			},
			hook: {
				bind: (value, { form }) => {
					return value.split("|").filter(Boolean); // 结果为：[1, 2, 3]
				},
				submit: (value, { form }) => {
					return value.join("|");
				}
			}
		},

		{
			label: "参与说明会",
			prop: "participationInstructions",
			flex: false,
			component: {
				name: "el-radio-group",
				options: [
					{ label: "愿意", value: "愿意" },
					{ label: "不愿意", value: "不愿意" },
					{ label: "未知", value: "未知" }
				]
			},
			value: "愿意"
		},
		{
			label: "消息来源",
			prop: "messageSource",
			component: { name: "el-input", props: { clearable: true } }
		},
		{ label: "备注", prop: "remark", component: { name: "cl-editor-wang" } }
	]
});

// cl-table
const Table = useTable({
	columns: [
		{ type: "selection" },
		{ label: "学生姓名", prop: "studentName", minWidth: 140 },
		{ label: "学生编号", prop: "studentId", minWidth: 140 },
		{ label: "学生性别", prop: "studentGender", minWidth: 140 },
		{ label: "学生身份证号", prop: "studentIdCard", minWidth: 140 },
		{ label: "学校编号", prop: "schoolId", minWidth: 140 },
		{
			label: "户籍地址",
			prop: "registeredResidenceAddress",
			minWidth: 160
		},
		{
			label: "通讯地址",
			prop: "studentAddress",
			minWidth: 160
		},
		{ label: "在学状态", prop: "studentStatus", dict: [], dictColor: true, minWidth: 120 },
		{ label: "同住家人", prop: "familyMembers", showOverflowTooltip: true, minWidth: 200 },
		{
			label: "特殊病史",
			prop: "specialMedicalHistory",
			showOverflowTooltip: true,
			minWidth: 200
		},
		{
			label: "备注",
			prop: "remark",
			minWidth: 120,
			component: { name: "cl-editor-preview", props: { name: "wang" } }
		},
		{ label: "消息来源", prop: "messageSource", minWidth: 140 },
		{
			label: "参与说明会",
			prop: "participationInstructions",
			minWidth: 100,
			component: { name: "cl-switch" }
		},
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
		service: service.studentpay.student_manager
	},
	(app) => {
		app.refresh();
	}
);
</script>
