<template>
  <div class="item">
    <a-upload
      style="margin-right: 8px"
      name="file"
      accept=".xlsx"
      :file-list="fileList"
      :multiple="false"
      :customRequest="readImportExcel"
      @change="handleChangeFileImport"
      :showUploadList="false"
      ref="fileupload"
    >
      <a-button type="primary">Chọn file</a-button>
    </a-upload>

    <a-table
      :columns="columns"
      :dataSource="dataTable"
      :pagination="pagination"
      :rowKey="(record, index) => record.key"
    >
    </a-table>
  </div>
</template>

<script>
import readXlsxFile from "read-excel-file";

export default {
  data() {
    return {
      fileList: [],
      dataTable: [],
      selectedRowKeys: [],
      showSelect: false,
      columns: [
        { title: "STT", dataIndex: "code", key: "code" },
        { title: "Số điện thoại", dataIndex: "phoneNumber", key: "phoneNumber" },
        { title: "Họ và tên", dataIndex: "custName", key: "custName" },
        { title: "Mã Bưu điện tỉnh", dataIndex: "codeProvince", key: "codeProvince" },
        { title: "Mã Bưu điện huyện", dataIndex: "codeDistrict", key: "codeDistrict" },
        { title: "Bưu cục (nếu có)", dataIndex: "codePost", key: "codePost" },
      ],
      pagination: {
        current: 1,
        pageSize: 5,
        total: 0,
        onChange: (page) => this.handlePageChange(page),
      },
    };
  },
  methods: {
    readImportExcel({ file, onSuccess, onError }) {
      this.dataTable = [];
      const schema = {
        STT: {
          prop: "stt",
          type: String,
        },
        "Số điện thoại": {
          prop: "phoneNumber",
          type: String,
        },
        "Họ và tên\n(đúng với thông tin trên GTTT)": {
          prop: "custName",
          type: String,
        },
        "Mã Bưu điện tỉnh": {
          prop: "codeProvince",
          type: String,
        },
        "Mã Bưu điện huyện": {
          prop: "codeDistrict",
          type: String,
        },
        "Bưu cục (nếu có)": {
          prop: "codePost",
          type: String,
        },
      };
      readXlsxFile(file, {
        schema,
        ignoreEmptyRows: true,
      })
        .then(({ rows, errors }) => {
          if (errors.length > 0) {
            for (let i = 0; i < errors.length; i++) {
              const err = errors[i];
              this.$notification["error"]({
                message: "Lỗi",
                description:
                  err.error === "required"
                    ? "Không được để trống các trường thông tin trong file import"
                    : err.error,
              });
            }
            this.fileList = [];
            this.dataTable = [];
            return;
          }
          if (rows.map((e) => "phoneNumber" in e).includes(false)) {
            this.$notification["error"]({
              message: "Lỗi",
              description: "File dữ liệu không hợp lệ",
            });
            this.fileList = [];
            this.dataTable = [];
            return;
          }
          if (rows.map((e) => "custName" in e).includes(false)) {
            this.$notification["error"]({
              message: "Lỗi",
              description: "File dữ liệu không hợp lệ",
            });
            this.fileList = [];
            this.dataTable = [];
            return;
          }
          if (rows.map((e) => "codeProvince" in e).includes(false)) {
            this.$notification["error"]({
              message: "Lỗi",
              description: "File dữ liệu không hợp lệ",
            });
            this.fileList = [];
            this.dataTable = [];
            this.isValidFile = false;
            return;
          }
          if (rows.map((e) => "codeDistrict" in e).includes(false)) {
            this.$notification["error"]({
              message: "Lỗi",
              description: "File dữ liệu không hợp lệ",
            });
            this.fileList = [];
            this.dataTable = [];
            return;
          }
          if (rows.map((e) => "codePost" in e).includes(false)) {
            this.$notification["error"]({
              message: "Lỗi",
              description: "File dữ liệu không hợp lệ",
            });
            this.fileList = [];
            this.dataTable = [];
            return;
          }
          this.dataTable = rows.map((e) => {
            return {
              code: e.stt,
              phoneNumber: e.phoneNumber,
              custName: e.custName,
              codeProvince: e.codeProvince,
              codeDistrict: e.codeDistrict,
              codePost: e.codePost,
            };
          });

          this.selectedRowKeys = this.dataTable.map((row) => row.code);
          this.pagination.total = rows.length;
          this.pagination.current = 1;
          this.showSelect = false;

          onSuccess();
        })
        .catch((error) => {
          console.error("🚀 ~ error:", error);
          onError();
        });
    },
    handlePageChange(page) {
      this.pagination.current = page;
    },
    handleChangeFileImport(info) {},
  },
};
</script>

<style scoped></style>

<style scoped></style>

<style scoped></style>
