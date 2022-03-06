# 文件流下载


````
  function downloadHandle() {
    return new Promise(resove => {
      let params = {
        reportId: this.$store.getter.reportId,
        templateName: "desKnownDefectUser",
      };
      this.$service.network.post("services/excelservices/downloadTemplate", params, {responseType: "blob"})
        .then(res => {
          const {data, status} = res || {};
          if (status === 200) {
            resove({fileData: data, fileName: "下载的模板.xlsx"});
          }
        });
    }).then(({fileData, fileName}) => {
      let f = document.createElement("iframe");
      document.body.appendChild(f);
      let blob = new Blob([fileDate], {
        //两种type都可以下载excel文件
        type: "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet;charset=UTF-8",
        // type: "application/vnd.ms-excel;charset=UTF-8",
      });
      let src = window.URL.createObjectURL(blob);
      f.src = src;
      setTimeout(() => {
        document.body.removeChild(f);
        URL.revokeObjectURL(src); //window下的方法都可以省略window
      }, 500);
    })
  }

````







