<template>
  <div>
    <ArtSearchBar
      ref="searchBarRef"
      v-model="searchFormState"
      :items="searchItems"
      :is-expand="false"
      :show-expand="true"
      :show-reset-button="true"
      :show-search-button="true"
      :disabled-search-button="false"
      :buttonLeftLimit="3"
      @search="handleSearch"
      @reset="handleReset"
    />

    <div class="page art-full-height">
      <ElCard class="art-table-card" shadow="never" style="margin-top: 0">
        <!-- 表格 -->
        <ArtTable
          rowKey="id"
          :loading="loading"
          :data="data"
          :columns="columns"
          empty-height="360px"
          :pagination="pagination"
          @pagination:size-change="handleSizeChange"
          @pagination:current-change="handleCurrentChange"
        >
          <template #operation="{ row }">
            <div class="operation-buttons">
              <ArtButtonTable type="view" @click="operDetail" :row="row">详情</ArtButtonTable>
            </div>
          </template>
        </ArtTable>
      </ElCard>
    </div>
  </div>
</template>

<script setup lang="ts">
  import { useTable } from '@/composables/useTable'
  import { fetchRechargeActList } from '@/api/rechargeActManage'

  defineOptions({ name: 'RechargeActRec' })

  // 表单搜索初始值
  const searchFormState = ref({
    phone: '',
    area: '',
    daterange: ['', '']
  })

  // 搜索表单配置
  const searchItems = computed(() => [
    {
      key: 'name',
      label: '用户名称',
      labelWidth: '100px',

      type: 'input',
      props: {
        placeholder: '请输入用户名称'
      }
    },
    {
      key: 'phone',
      label: '用户电话',
      labelWidth: '100px',

      type: 'input',
      props: {
        placeholder: '请输入用户电话'
      }
    },
    {
      key: 'area',
      label: '专区',
      type: 'select',
      labelWidth: '100px',

      props: {
        placeholder: '请选择',
        options: []
      }
    },
    {
      key: 'actRec',
      label: '活动记录ID',
      labelWidth: '100px',
      type: 'input',
      props: {
        placeholder: '请输入活动记录ID'
      }
    },
    {
      key: 'rechargeActId',
      label: '充值活动ID',
      labelWidth: '100px',

      type: 'input',
      props: {
        placeholder: '请输入充值活动ID'
      }
    },
    {
      key: 'rechargeActId',
      label: '支付记录ID',
      labelWidth: '100px',

      type: 'input',
      props: {
        placeholder: '请输入支付记录ID'
      }
    },
    {
      key: 'energyRecId',
      label: '能量记录ID',
      labelWidth: '100px',

      type: 'input',
      props: {
        placeholder: '请输入能量记录ID'
      }
    },
    {
      label: '创建时间',
      key: 'daterange',
      type: 'datetime',
      labelWidth: '90px',
      props: {
        type: 'daterange',
        valueFormat: 'YYYY-MM-DD',
        rangeSeparator: '至',
        startPlaceholder: '开始日期',
        endPlaceholder: '结束日期'
      }
    }
  ])

  const handleSearch = async () => {
    const { daterange, ...filtersParams } = searchFormState.value
    const [startTime, endTime] = Array.isArray(daterange) ? daterange : [null, null]

    // 搜索参数赋值
    Object.assign(searchParams, { ...filtersParams, startTime, endTime })
    getData()
  }

  const handleReset = () => {
    resetSearchParams()
  }
  const operDetail = () => {}

  const {
    data,
    columns,
    loading,
    pagination,
    searchParams,
    resetSearchParams,
    handleSizeChange,
    handleCurrentChange,
    getData
  } = useTable({
    core: {
      apiFn: fetchRechargeActList,
      apiParams: {
        page: 1,
        size: 20
      },
      columnsFactory: () => [
        {
          prop: 'id',
          label: '#'
        },
        {
          prop: 'name',
          label: '活动记录ID',
          width: 200,
          align: 'center'
        },
        {
          prop: 'area',
          label: '专区'
        },
        {
          prop: 'userName',
          label: '用户名称'
        },
        {
          prop: 'phone',
          label: '用户电话'
        },
        {
          prop: 'rechargeAmount',
          label: '充值金额'
        },
        {
          prop: 'giveEnergyAmount',
          label: '赠送能量'
        },
        {
          prop: 'limitTimes',
          label: '限制次数'
        },
        {
          prop: 'createTime',
          label: '创建时间'
        },
        {
          prop: 'rechargeActId',
          label: '充值活动ID',
          width: 190,
          align: 'center'
        },
        {
          prop: 'payRecordId',
          label: '支付记录ID',
          width: 100,
          align: 'center'
        },
        {
          prop: 'energyRecId',
          label: '能量记录ID',
          width: 180,
          align: 'center'
        },
        {
          prop: 'operation',
          label: '操作',
          width: 120,
          useSlot: true,
          fixed: 'right'
        }
      ]
    }
  })
</script>

<style lang="scss" scoped></style>
