<template>
  <q-page class="constrain-more q-pa-md">
    <div class="camera-frame q-pa-md">
      <video v-show="!imageCaptured" ref="video" class="full-width" autoplay />
      <canvas v-show="imageCaptured" ref="canvas" class="full-width" height="240" />
    </div>
    <div class="text-center q-pa-md">
      <q-btn
        v-if="hasCameraSupport"
        @click="captureImage"
        color="grey-10"
        size="20px"
        icon="eva-camera"
        round
      />
    <q-file v-else
    outlined
    @update:model-value="captureImageFallBack"
    accept="image/*"
    v-model="imageUpload"
    label="Choose an Image">
      <template v-slot:prepend>
        <q-icon name="eva-attach-outline" />
      </template>
    </q-file>
      <div class="justify-center q-ma-md">
        <q-input
          v-model="post.caption"
          class="col col-sm-6"
          label="caption"
          dense
        />
        <q-input
          v-model="post.location"
          class="col col-sm-6"
          label="location"
          dense
        >
          <template v-slot:append
            ><q-btn
            @click="getLocation"
            round
            dense
            flat
            icon="eva-navigation-2-outline"
          /></template>
        </q-input>
      </div>
      <div class="row justify-center q-mt-lg">
        <q-btn color="primary" label="Post image" rounded unelevated />
      </div>
    </div>
  </q-page>
</template>

<script>
import { uid } from "quasar";
import axios from 'axios'
require("md-gum-polyfill");
export default {
  name: "PageCamera",
  data() {
    return {
      post: {
        id: uid(),
        catpion: "",
        location: "",
        photo: null,
        date: Date.now(),
      },
      imageCaptured:false,
      imageUpload:[],
      hasCameraSupport:true
    };
  },
  methods: {
    initCamera() {
      //console.log("initCamera");
      navigator.mediaDevices
        .getUserMedia({
          video: true,
        })
        .then((stream) => {
          this.$refs.video.srcObject = stream;
        }).catch(error=>{
          this.hasCameraSupport = false;
        });
    },
    captureImage() {
      console.log("captureImage");
      let video=this.$refs.video;
      let canvas=this.$refs.canvas;
      canvas.width=video.getBoundingClientRect().width;
      canvas.height=video.getBoundingClientRect().height;
      let context=canvas.getContext('2d');
      context.drawImage(video,0,0,canvas.width,canvas.height)
      this.imageCaptured=true
      this.post.photo=this.dataURItoBlob(canvas.toDataURL())
      this.disableCamera()
    },
    captureImageFallBack(file){
      this.post.photo=file;

      let canvas=this.$refs.canvas;
      let context=canvas.getContext('2d');

      var reader=new FileReader();
      reader.onload=event=>{
        var img= new Image();
        img.onload=()=>{
          canvas.width=img.width
          canvas.height=img.height
          context.drawImage=(img,0,0)
          this.imageCaptured=true
        }
        img.src=event.target.result;
      }

      reader.readAsDataURL(file)
    },
    dataURItoBlob(dataURI){
      var byteString=atob(dataURI.split(',')[1]);
      var mimeString=dataURI.split(',')[0].split(':')[1].split(';')[0]
      var ab=new ArrayBuffer(byteString.length);
      var ia=new Uint8Array(ab);
      for(var i=0;i<byteString.length;i++){
        ia[i]=byteString.charCodeAt(i);
      }
      var blob = new Blob([ab],{type:mimeString.length});
      return blob
    },
    disableCamera(){
      this.$refs.video.srcObject.getVideoTracks().forEach(track=>{
        track.stop()
      })
    },
    getLocation(){
      navigator.geolocation.getCurrentPosition(position=>{
        this.getCityAndCountry(position)
      },err=>{
        this.locationError(err)
      },{timeout:7000})
    },
    async getCityAndCountry(position){
      try{
      let apiURL = `https://geocode.xyz//${position.coords.latitude},${position.coords.longitude}?json=1`;
      const res=await axios.get(apiURL)
      console.log(`res: `,res)
       this.locationSuccess(res);
      }catch(error){
        this.locationError(error)
      }
    },
    locationSuccess(result){
      this.post.location=result.data.city
      if(result.data.city){
        this.post.location+=`, ${this.data.country}`
      }
    },
    locationError(){
      this.$q.dialog({
        title:'Alert',
        message:'Could not find your location'
      })
    }
  },
  mounted() {
    this.initCamera();
  },
  beforeUnmount(){
    if(this.hasCameraSupport)
    this.disableCamera();
  }
};
</script>
<style lang="scss">
.camera-frame {
  border: 1px solid;
  color: "grey-10";
  border-radius: 10px;
}
</style>
