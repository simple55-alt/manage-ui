<template>
  <div id="returnbooks">
    <Card>
      <Button
        type="primary"
        class="m-b-10"
        @click="returnBook"
      >还书</Button>
      <!-- form内弹出窗 -->
      <Modal
        v-model="formMdelVisible"
        :mask-closable="false"
        title="借书列表"
        width="80%"
      >
        <boorowbooks
          :choiceVisible="true"
          @choice="boorowChoice"
        ></boorowbooks>
        <template slot="footer">
        </template>
      </Modal>
      <!-- 还书弹窗 -->
      <Modal
        v-model="returnVisible"
        :mask-closable="false"
        title="借书"
      >
        <Form
          ref="tableFormInline"
          :disabled="modelTitle==='借书查看'"
          :model="tableForm"
          :label-width="100"
        >
          <FormItem
            prop="bookName"
            label="所借图书"
          >
            <div
              @click="boorowFocus"
              style="border: 1px solid #dcdee2;height: 24px;border-radius: 3px;line-height: 24px;padding-left: 5px;"
            >
              {{tableForm.bookName}}
            </div>
          </FormItem>
          <FormItem
            prop="returnName"
            label="还书人姓名"
          >
            <Input
              type="text"
              v-model="tableForm.returnName"
              placeholder="还书人姓名"
            ></Input>
          </FormItem>
          <FormItem
            prop="overdue"
            label="是否超期"
          >
            <Select
              v-model="tableForm.overdue"
              placeholder="是否超期"
            >
              <Option
                v-for="item in whether"
                :value="item.value"
                :key="item.value"
              >{{ item.label }}</Option>
            </Select>
          </FormItem>
          <FormItem
            prop="overtimeMoney"
            label="超时扣费"
          >
            <Input
              type="text"
              v-model="tableForm.overtimeMoney"
              placeholder="超时扣费"
            ></Input>
          </FormItem>

          <FormItem
            prop="overtimeDays"
            label="超时天数"
          >
            <Input
              type="text"
              v-model="tableForm.overtimeDays"
              placeholder="超时天数"
            ></Input>
          </FormItem>
        </Form>

        <template slot="footer">
          <Button
            type="primary"
            @click="returnVisible = false"
          >取消</Button>
          <Button
            type="primary"
            @click="returnOk"
          >确定</Button>
        </template>
      </Modal>
      <Table
        stripe
        :columns="tableCol"
        border
        :data="tableData"
      >
        <template
          slot="action"
          slot-scope="{ row, index }"
        >
          <Button
            type="primary"
            style="margin-right: 5px"
            @click="getDetails(row)"
          >查看</Button>
        </template>
        <Switch v-model="tableLoading"></Switch>
      </Table>
      <div>
        <Page
          class-name="page"
          :page-size="page.size"
          :total.sync="page.total"
          show-total
          @on-change="pageChange"
          @on-page-size-change="pageSizeChange"
        />
      </div>
    </Card>
    <!-- 查看、编写详情框 -->
    <Modal
      v-model="modelVisible"
      :mask-closable="false"
      :title="modelTitle"
    >
      <Form
        ref="tableFormInline"
        :disabled="modelTitle==='还书查看'"
        :model="tableForm"
        :label-width="100"
      >
        <Row>
          <Col span="9">
          <div>
            <img :src="tableForm.frontImageUrl" />
            <span>《{{tableForm.bookName}}》</span>
          </div>
          </Col>
          <Col span="15">
          <FormItem
            prop="readerName"
            label="借书人"
          >
            <Input
              type="text"
              v-model="tableForm.readerName"
              placeholder="借书人"
            ></Input>
          </FormItem>
          <FormItem
            prop="readerSex"
            label="借书人性别"
          >
            <Input
              type="text"
              v-model="tableForm.readerSex"
              placeholder="借书人性别"
            ></Input>
          </FormItem>
          <FormItem
            prop="borrowTime"
            label="借书日期"
          >
            <Input
              type="text"
              v-model="tableForm.borrowTime"
              placeholder="借书日期"
            ></Input>
          </FormItem>
          <FormItem
            prop="expectReturnTime"
            label="预计归还日期"
          >
            <Input
              type="text"
              v-model="tableForm.expectReturnTime"
              placeholder="预计归还日期"
            ></Input>
          </FormItem>
          <FormItem
            prop="returnTime"
            label="实际归还日期"
          >
            <Input
              type="text"
              v-model="tableForm.returnTime"
              placeholder="实际归还日期"
            ></Input>
          </FormItem>
          <FormItem
            prop="overdue"
            label="是否超期"
          >
            <Input
              type="text"
              v-model="tableForm.overdue"
              placeholder="是否超期"
            ></Input>
          </FormItem>
          <FormItem
            prop="overtimeMoney"
            label="超时扣费"
          >
            <Input
              type="text"
              v-model="tableForm.overtimeMoney"
              placeholder="超时扣费"
            ></Input>
          </FormItem>
          <FormItem
            prop="overtimeDays"
            label="超时天数"
          >
            <Input
              type="text"
              v-model="tableForm.overtimeDays"
              placeholder="超时天数"
            ></Input>
          </FormItem>
          </Col>
        </Row>
      </Form>
      <template slot="footer">
        <Button
          type="primary"
          @click="modelVisible = false"
        >取消</Button>
      </template>
      <Switch v-model="formLoading"></Switch>
    </Modal>
  </div>
</template>
<script src="./index.js"></script>
<style src="./index.css" scoped></style>