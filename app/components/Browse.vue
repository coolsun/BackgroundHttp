<template lang="html">
    <Page>
        <ActionBar>
            <Label text="Browse"></Label>
        </ActionBar>

        <GridLayout class="page__content">
            <Label class="page__content-icon far" text.decode="&#xf1ea;"></Label>
            <Label class="page__content-placeholder" :text="message"></Label>
        </GridLayout>
    </Page>
</template>

<script>
  const bgHttp = require("@nativescript/background-http");
  export default {
    data() {
      return {
        description: "test @nativescript/background-http",
        message: "<!-- Browse page content goes here -->"
      };
    },
    created() {
      this.getData();
    },
    methods: {
      getData() {
        var self = this;
        let request = {
            url: "http://35.236.145.187:8200/apis/worksheets/${this.worksheetID}/upload_maintaining",
            method: "POST",
            headers: {
                "Content-Type": "application/octet-stream"
            },
            description: "Uploading image..."
        };
        request['worksheet[description]']= this.description;
        let params = [
          { name: 'worksheet[description]', value: this.description }
        ]
        var bgHttpSession = bgHttp.session(`${new Date().getTime()+Math.random()}`);
        let task = bgHttpSession.multipartUpload(params, request);
        task.on("error", (event) => {
          confirm({
              title: "Whoops",
              okButtonText: "OK",
              message: "Something went wrong."
          });
        });

        task.on("responded", (event) => {
          confirm({
                title: "Responded",
                okButtonText: "OK",
                message: "Responded: files uploaded."
          })
        });
      }
    }
  }
</script>

<style lang="scss" scoped>
    // Start custom common variables
    @import "@nativescript/theme/scss/variables/blue";
    // End custom common variables

    // Custom styles

</style>
