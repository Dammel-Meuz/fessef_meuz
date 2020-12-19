<template>
  <div class="container">
    <div class="row">
      <div class="col-md-2"></div>

      <div class="col-md-8">
        <div class="card feseul_card post_div" id="">
          <H5 class="card-header">
            <img
              class="rounded-circle border border-warning d-inline"

              alt="user"
              width="50px"
              height="50px" />

            <small class="text-muted"></small
          ></H5>

          <img
            v-if="
              this.feusseul[0].extension == 'jpeg' ||
              this.feusseul[0].extension == 'jpg'  ||
              this.feusseul[0].extension == 'npg'
            "
            class="card-img"
            :src="'/uploads/' + this.feusseul[0].file"
            alt="image"
          />
           <video
            controls
            height="400px"
            v-if="this.feusseul[0].extension == 'mp4'||this.feusseul[0].extension == 'ogg'"
            width="1000px"
          >
            <source
              :src="'/uploads/videos/' + this.feusseul[0].file"
              type="video/mp4"
            />
          </video>
          <div class="card-body">

            <p class="text-center p-4">
            <strong>{{ this.feusseul[0].titre }}</strong>
          </p>
          <p>
            {{ this.feusseul[0].contenu }}
          </p>
          </div>

          <div class="card-footer text-muted text-center">
            <div class="row">
              <div class="col-md-3">
                <p class="float-left d-inline ml-2"></p>
              </div>

              <div class="col-md-2">
                <p class="text-center d-inline ml-2"></p>

                <div id="like-section">
                <div class="row ">
                 <span v-if="this.feusseul[0].like != null">total like {{
                this.feusseul[0].like
                 }}</span>
                <span v-if="this.feusseul[0].like == null"></span>

                 <inertia-link :href="'/Feusseul/like/' + this.feusseul[0].id">
                 <i class="btn btn-sm btn-info">J'aime</i>
                </inertia-link>
                </div>
                </div>

              </div>

              <div class="col-md-2">
                <p class="float-right d-inline total"></p>
              </div>

              <!-- <div class="col-md-2">
                <p class="float-right d-inline total">Vue(s)</p>
              </div> -->
            </div>
          </div>
        </div>

        <!-- <div class="main-comment-section">

                </div>  -->

        <hr />

        <div class="row my-4"
         v-for="item in commentaires"
            :key="item.id"
        >
          <div class="col-md-1"

          >
            <img
              class="rounded-circle mx-auto d-block p-2"
              :src="'/uploads/avatar/'+item.user.avatar"
              alt=""
              style="width: 70px; height: 70px"
            />

          </div>
          <div class="col-md-11 mt-4" style="padding-left: 25px;">
            <b>{{ item.commentaires }} </b><br>
            <small class="">
              <small>De : </small>
              <strong>{{ item.user.name }}</strong>
            </small>
            <span class="badge badge-secondary"> {{ item. created_at }}</span>

            <div class="reply mx-0">
              <a
                class="text-secondary"
                data-toggle="collapse"
                href=""
                role="button"
                aria-expanded="false"
                aria-controls=""
              >
                <small>Repondre</small>
              </a>

              <div class="collapse" id="">
                <div class="card card-body my-2">
                  <form action="" method="POST" class="form-inline">
                    <div class="form-group my-2">
                      <textarea
                        class="form-control"
                        name="comment"
                        id=""
                        cols="60"
                        rows="1"
                        placeholder="Repondre..."
                      ></textarea>
                      <!-- <input type="text" class="form-control"  name="comment" placeholder="Repondre..."> -->
                      <input type="hidden" name="parentId" value="" />
                    </div>
                    <input type="hidden" name="postId" value="" />
                    <button
                      type="submit"
                      class="btn btn-primary btn-sm m-2"
                      id="btnRepondre"
                    >
                      Repondre
                    </button>
                  </form>
                </div>
              </div>
            </div>

          </div>
        </div>

        <hr />

        <form  @submit.prevent="handleSubmit" class="form-inline">
          <div class="form-group my-2">
            <textarea
              class="form-control"
              id="description"
              name="commentaires"
              cols="80"
              rows="1"
               v-model="form.commentaires"
              placeholder="Commentaire..."
            ></textarea>
            <!-- <input type="text" class="form-control"  name="comment" placeholder="Commentaire..."> -->
          </div>
          <input type="hidden" name="parentId" value="" />
          <input type="hidden" name="postId" value="" />
          <button type="submit" class="btn btn-primary m-2">Comment</button>
        </form>

        <hr />
      </div>

      <div class="col-md-2"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Show",
  props: ["feusseul", "commentaires"],
  data() {
    return {
      form: {
        commentaires: null,
        feusseul_id: this.feusseul[0].id,
      },
    };
  },
  methods: {
    handleSubmit() {
      const data = new FormData();

      data.append("commentaires", this.form.commentaires);
      data.append("feusseul_id", this.form.feusseul_id);

      this.$inertia.post("/Feusseul/comments", data, {
        onSuccess: () => {
          this.form.commentaires = "";
          this.flashMessage.info({
            message: "Commentaire poster avec success",
          });
        },
      });
    },
  },
};
</script>

<style>
</style>
