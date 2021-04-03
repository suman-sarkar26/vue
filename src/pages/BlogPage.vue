<template>
  <div class="bg-dark">
      <div class=" container bg-light">

        <div class="pl-5 bg-light">
          
        <!-- Blog section -->
        

        <div class="row pt-5">
          <div class="col-md-9">
            <div class="text">
              <h3 >{{details.title}}</h3>
              <h4 class="pb-3 pt-1">{{ details.created_by }} </h4>
              <p class="pb-4" v-for="(detailData, index) in detailInfo " :key="index"> {{  detailData.blog_detail}} </p>
            </div>
          </div>
          <div class="col-md-3">
            <span v-for="(detailData, index) in detailInfo " :key="index">
              <font-awesome-icon :icon="['fas', 'clock']" class="regular text-secondary"/>
               {{ detailData.blog_datetime }} 
            </span>
          </div>
        </div>

          <!-- Comments Section -->


        <div class="row" v-for="(commentData, index) in comments" :key="index">
          <div class="col col-md-9 pb-5">
            <h6>
              <font-awesome-icon :icon="['fas', 'user-friends']" class="solid text-primary"/>
              {{ commentData.user }} 
            </h6>
            <p>{{ commentData.comment }}</p>
            <font-awesome-icon :icon="['fas', 'trash']" type="button" @click="deleteComment(commentData.id)" class="solid text-danger"/>
          </div>
          <div class="col col-md-3">
            
            <p>
              <font-awesome-icon :icon="['fas', 'clock']" class="regular text-secondary"/>
              {{ commentData.comment_datetime }}
            </p>
          </div>
        </div> 

      <!-- New Comments Section -->

     <div class="row">
        <div class="col col-md-3">
          <form @submit.prevent="submitData()">
          <div class="form-group ">
            <input  class="form-control" type="text" v-model="commentInfo.user" placeholder="user name">
            
            <textarea class="form-control mt-3" v-model="commentInfo.comment" placeholder="Leave a comment"></textarea>
            
            <input type="submit" class="btn btn-success mt-3" value="Submit">
          </div>
          </form>
        </div>
      </div>
        </div>

    </div>
    
  </div>
</template>


<script>
import axios from 'axios';
import moment from 'moment';
  export default{
    data(){
      return{
        details:[],
        detailInfo:[],
        comments:[],
        commentInfo: {
          "id": null,
          "blog_id": null,
          "comment":null,
          "comment_datetime":"",
          "user":null
        }
       }
        
       
    },
  methods: {
  
     async submitData(){
       this.commentInfo.blog_id = this.$route.params.allData.id;
       let date = new Date().toLocaleString();
       this.commentInfo.comment_datetime = moment(date).format('YYYY-MM-DD h:mm:ss'); 

      await axios.post("http://vue-test.gingerbd.com/api/post-comment",
         this.commentInfo);
      },

       async deleteComment(id){
        await axios.get("http://vue-test.gingerbd.com/api/remove-comment/"+id);
        this.created();
        // console.log("comment deleted id:");
        // console.log(id);
        
      }
  },
  created(){
       
    this.comments =  this.$route.params.allData.all_comments;
    this.details =  this.$route.params.allData;
    this.detailInfo =  this.$route.params.allData.all_blogs;
  }
}
</script>
