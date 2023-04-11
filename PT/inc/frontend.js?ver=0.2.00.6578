

///////////////////// MY AREA    ///////////////////////

function set_vis(div_id) {}
function set_btn(btn,selected) {}
function set_btns(active_id) {}

function pxl(id,comment="XXX") {
      if (id>0) { // (id>last_pixel) {
          executeAsync(function() {
              var ajax = new XMLHttpRequest();
              ajax.open("POST", "pxl", true);
              ajax.onreadystatechange = function() {
                if(ajax.readyState === XMLHttpRequest.DONE) {
                    last_pixel = id;
                    // alert("pixel sent: "+id);
                }
              }
              ajax.setRequestHeader("Content-Type", "application/upload");
              ajax.send("?id="+id+"&r="+comment+"&a=1");
          },0);
      }
}


var images = [null,null,null,null,null,null,null,null,null,null,null,null,null,null,null];
var share_urls = ["#","#","#","#","#","#","#","#","#","#","#","#"];
var last_filter=0;
var send_snap=1;

const video1 = document.querySelector('#videoCam');
video1.muted=true;
//video1.play();

const img = document.querySelector('#screenshot-img');
const desc_text = document.querySelector('#desc_text');
const canvas = document.createElement('canvas');

function resetFilters() {
    thumbs.slideTo(0,0, false);
    var imgs = $('#filter_gallery img');
    for (var i=0;i<imgs.length;i++) {
        imgs[i].src = "/PT/art/filter/black.jpg";
    }
    images = [null,null,null,null,null,null,null,null,null,null,null,null,null,null,null];
    share_urls = ["#","#","#","#","#","#","#","#","#","#","#","#"];

    for (var i=1;i<5;i++) {
        var loading = $("#loading_slide_"+i)
        if (typeof(loading) != 'undefined' && loading != null && loading.length>0)
            loading[0].style.display="block";
    }

}
function setFilter(btnId,filterId,bg_mask) {
  if (filterId=="") filterId=0
  if (filterId==-1) {
    set_vis(1);
    desc_text.innerHTML ="";
  } else {
      set_vis(2);
      if (images[filterId]!=null) {
        img.src = images[filterId];
        set_btns(btnId);
        set_desc_text(filterId);

        setTimeout(function(){
            if (filterId==0){
                $('#back').animate({ opacity: 1, }, 500);
//                $('#filtercontrol,#back').animate({ opacity: 1, }, 500);
            }else {
                $('#back,#share_nav').animate({ opacity: 1, }, 500);
//                $('#filtercontrol,#back,#share_nav').animate({ opacity: 1, }, 500);
                $("#info_toggle,#share").fadeIn(500);
            }
        }, 900);
        updateShare(share_urls[filterId]);

      } else {
          set_btns(-1);
          var canvasData = images[0]; //canvas.toDataURL("image/png");
          // img.src = images[last_filter];
          // var img2 = $('#filter_gallery .swiper-slide-active img');
          // img2[0].src = images[last_filter];

          var filterImgs = $('#filter_gallery .swiper-slide-active img');
          var filterImg = filterImgs[0].id;

          executeAsync(function() {
              var ajax = new XMLHttpRequest();
              ajax.open("POST", "web_snapshot", true);
              ajax.timeout=30000;
              ajax.onreadystatechange = function() {
                if(ajax.readyState === XMLHttpRequest.DONE) {
//                    set_btns(btnId);
//                    console.log(ajax.responseText);
//                    var img = $('#filter_gallery .swiper-slide-active img'); //.attr('src');
                    var img = $('#'+filterImg); //.attr('src');

                    var ajax_response = ajax.responseText.split('$');
                    var share_url = ajax_response[0];
                    img[0].src = images[filterId] = ajax_response[1];// ajax.responseText;

                    var loadingId = filterImg.replace("img_","loading_");
                    var loading = $("#"+loadingId);
                    if (typeof(loading) != 'undefined' && loading != null && loading.length>0)
                        loading[0].style.display="none";

                    last_filter = filterId;
//                    alert('set filter post finished:\n'+ajax.responseText)
                    var descDivId = filterImg.replace("img_","desc_");
                    set_desc_text(filterId,descDivId);

                    share_urls[filterId] = share_url;
                    setTimeout(function(){
                        $('#back,#share_nav').animate({ opacity: 1, }, 500);
//                        $('#filtercontrol,#back,#share_nav').animate({ opacity: 1, }, 500);
                        $("#info_toggle,#share").fadeIn(500);
                    }, 900);
                    updateShare(share_url);
                }
              }
              ajax.setRequestHeader("Content-Type", "application/upload");
              ajax.send("?first="+send_snap+"&filter_id="+filterId+"&bg_mask="+bg_mask+"&img="+canvasData);
              send_snap=0;

          },0);
          pxl(3,filterId);
      }
  }
}

function set_desc_text(filterId,divId) {
      var ajax2 = new XMLHttpRequest();
      ajax2.open("POST", "web_desc_text", true);
      ajax2.timeout=30000;
      ajax2.onreadystatechange = function() {
        if(ajax2.readyState === XMLHttpRequest.DONE) {
//            alert("set response text: "+ajax2.responseText);
//            var divs = $('#filter_gallery .swiper-slide-active div'); //.attr('src');
            var divs = $('#'+divId); //+' div'); //.attr('src');
            if (divs.length > 0)
                divs[0].innerHTML =ajax2.responseText;
        }
      }
      ajax2.send("?filter_id="+filterId+"&ignore=0");
}

function executeAsync(func,milli) {
    setTimeout(func, milli);
}
function handleSuccess(stream) {
//  screenshotButton.disabled = false;
//  video.srcObject = stream;
}

//// set_vis(1);
//last_pixel = 0 ;
pxl(1);


//################# DESIGN  ####################

const documentHeight = () => {
    const doc = document.documentElement
    doc.style.setProperty('--doc-height', `${window.innerHeight}px`)
   }
   window.addEventListener('resize', documentHeight)
   documentHeight()

const swiper1 = new Swiper('.swiper-container-1', {
    loop: false,
    effect: 'fade',
    speed: 500,
    autoplay: {
        delay: 400,//900,
        stopOnLastSlide: true
      },
});   
const swiper2 = new Swiper('.swiper-container-2', {
    loop: true,
    effect: 'fade',
    speed: 500,
    autoplay: {
        delay: 1200,
      },
});   
const filterGallery = new Swiper('.swiper-container-3', {
    slidesPerView: 1,
    centeredSlides: true,
    speed: 600,
    loop: false,
    loopedSlides: 1,
});
const thumbs = new Swiper('.swiper-container-4', {
    slidesPerView:  'auto',
    spaceBetween: 8,
    centeredSlides: true,
    loop: false,
    slideToClickedSlide: true,
});

filterGallery.controller.control = thumbs;
thumbs.controller.control = filterGallery;

var imgURL;

(function($) {
	$(document).ready(function(){

        // intro animation:
        $('.swiper-container-1').addClass('slideUP');

        // screen 2 button:
		$('#next_screen').on('click', function(e) {
            //button:
            $(this).toggleClass('shrink');
			$(this).fadeOut( 250, function() {
                $('#permit_camera').toggleClass('shrink').fadeIn(250);
            });
            //change text:
            $('#sc-02').removeClass('visible');
            setTimeout(function(){
                $('#sc-03').addClass('visible');
            }, 900);
		});	

        // open camera screen:
        $('#permit_camera').on('click', function(e) {
            swiper2.autoplay.stop();
            $('#s-02').removeClass('active');
            $('#step1').addClass('active');
        });	

        // move to filter gallery screen:
        $('#screenshot-button').on('click', function(e) {
            $('#step1').removeClass('active');
            $('#step2').addClass('active');
            setTimeout(function(){
                $('.timer-hide').toggleClass('hidden');
            }, 1000);
        });	 
        $('#flipcamera').on('click', function(e) {
            cameraIdx++;
            if (cameraIdx>=all_cameras.length) cameraIdx=0;
            openCam();
        });
        $('#back').on('click', function(e) {
            resetFilters();

            $('#step2').removeClass('active');
            $('#step1').addClass('active');
        });
        $('#share').on('click', function(e) {
            var filterId = document.querySelector('#filter_gallery .swiper-slide-active img').alt;
            shareImage(filterId);
//            $('#share_nav').toggleClass('closed');
//            $(this).find('.icon-close').toggleClass('vis');
//            $(this).find('.icon-share').toggleClass('vis');
        });

        // open info ticket:
        $('#info_toggle').on('click', function(e) {
            $('#filter_gallery .swiper-slide-active').find('.filter_info').addClass('open_card');
            $('#filtercontrol').fadeOut(200);
            $("#info_toggle").fadeOut(200);
            $('#share,#share_nav,#back').css({
                'opacity': '0',
                'visibility': 'hidden'
            });
        });	
        // close info ticket:
        $('.close_info').on('click', function(e) {
            $(this).parents('.filter_info').removeClass('open_card');
            setTimeout(function(){
                $('#filtercontrol').fadeIn(500);
                $("#info_toggle").fadeIn(500);
                $('#share,#share_nav,#back').css({
                    'opacity': '1',
                    'visibility': 'visible'
                });
            }, 700);
        });	
        
    });

    // change to first screen after intro:
    swiper1.on('reachEnd', function(){
        setTimeout(function(){
            $('#s-01').removeClass('active');
            $('#s-02').addClass('active');
        }, 1200);
        setTimeout(function(){
            $('#sc-02').addClass('visible');
            $('#next_screen').addClass('scale');
        }, 1600);
    });
    // change to first screen on intro click:
    $('#intro-swiper').on('click', function(e) {
        swiper1.autoplay.stop();
        $('#s-01').removeClass('active');
        $('#s-02').addClass('active');
        setTimeout(function(){
            $('#sc-02').addClass('visible');
            $('#next_screen').addClass('scale');
        }, 600);
    });	 

    // slide between filter images:
    thumbs.on("slideChange", function() {
        if(this.realIndex != 0){      
            $("#info_toggle,#share").fadeOut(150);
            $('#back,#share_nav').css('opacity', '0');
//            $('#filtercontrol,#back,#share_nav').css('opacity', '0');
        } else {
            $("#info_toggle,#share").fadeOut(150);
            $('#share_nav').css('opacity', '0');
        }
    });
    filterGallery.on("slideChangeTransitionEnd", function() {
        var imgURL = $('#filter_gallery .swiper-slide-active img').attr('src');
        var imgALT = $('#filter_gallery .swiper-slide-active img').attr('alt');
        var index = thumbs.realIndex;
        var maskBG = 1;
        setFilter(imgALT,imgALT,maskBG);
//        updateShare();
        console.log(imgURL);
    });

})(jQuery);

var sharedImgUrl = '#';

function updateShare(share_url){
    var imgURL = document.querySelector('#filter_gallery .swiper-slide-active img').src;
    var encImgURL =  encodeURIComponent(imgURL);
    sharedImgUrl =  share_url;
    $("#facebook_share").attr("href", "https://www.facebook.com/sharer/sharer.php?u="+share_url);
    $("#download_img").attr("href", imgURL);
    console.log(imgURL);
}
function openWhatsApp() {
    try{
        var imgURL = document.querySelector('#filter_gallery .swiper-slide-active img').src;
        var filterId = document.querySelector('#filter_gallery .swiper-slide-active img').alt;
        var encImgURL =  encodeURIComponent(imgURL);
    //    sysShare(51);
        shareImage(filterId);
         //window.open('whatsapp://send?text='+sharedImgUrl);
    } catch (error) {
        console.error(error);
    }
}


var facingMode = 'user';// 'environment';
var cameraIdx = 0;
//var all_cameras = null;
//navigator.mediaDevices.enumerateDevices()
//.then(function(devices) {
//    all_cameras = devices.filter(function(device) {
//      return device.kind === "videoinput";
//    });
//    });

// Video code:

// OLD Video code:
function openCam(){
    var All_mediaDevices = navigator.mediaDevices;
    if (!All_mediaDevices || !All_mediaDevices.getUserMedia) {
        alert("getUserMedia() not supported.");
       console.log("getUserMedia() not supported.");
       return;
    }
    var video = document.getElementById('videoCam');
    if (video.srcObject!=null) {
        video.srcObject.getTracks().forEach(function(track) {
//            alert("opencam 8: stop track");
            track.stop();
        });
    }
    video.muted=true;
    video.play();
    All_mediaDevices.getUserMedia({
//       audio: false,
       video: true //,
       //facingMode: facingMode
    })
    .then(function(vidStream) {
       video.srcObject = vidStream;
       if ("srcObject" in video) {
          video.srcObject = vidStream;
       } else {
          video.src = window.URL.createObjectURL(vidStream);
       }
       video.onloadedmetadata = function(e) {
          video.muted=true;
          video.play();
       };
//        alert("open cam 3: vidplay");
//        video.muted=true;
//        video.play();
    })
    .catch(function(e) {
       console.log(e.name + ": " + e.message);
       alert("stream err:"+e.name + ": " + e.message)
    });
}
function stopStreamedVideo() {
    takepicture();
    var video = document.getElementById('videoCam');
    const stream = video.srcObject;
    const tracks = stream.getTracks();

    tracks.forEach((track) => {
      track.stop();
    });

    video.srcObject = null;
}

//
// video capture
//
const width = window.innerWidth; //320; // We will scale the photo width to this
const height = window.innerHeight; // This will be computed based on the input stream

function clearphoto() {
//    alert("clearphoto");
    const context = canvas.getContext("2d");
    context.fillStyle = "#ccc";
    context.fillRect(0, 0, canvas.width, canvas.height);

    const data = canvas.toDataURL("image/png");
    photo.setAttribute("src", data);
  }

function takepicture() {
    var video = document.getElementById("videoCam");
    const context = canvas.getContext("2d");
    if (width && height) {
      canvas.width = video.videoWidth;
      canvas.height = video.videoHeight;
      canvasContext = canvas.getContext('2d');
      canvasContext.translate(canvas.width , 0);
      canvasContext.scale(-1, 1);
      canvasContext.drawImage(video, 0, 0);

      // Other browsers will fall back to image/png
      images = [canvas.toDataURL('image/webp'),null,null,null,null]
      img.src = images[0];

      send_snap=1
      setFilter(0,0);
      last_filter=0;
      pxl(2);
    } else {
      clearphoto();
    }
}


function shareImage(filterId) {
//  const response = await fetch(url);
  try{
        var response = images[filterId].substring(22);
        var byts = _base64ToArrayBuffer(response);
//      const blob = await response.blob();
      const filesArray = [
        new File(
          [byts],
          'ptm-selfie.png',
          {
            type: "image/png",
            lastModified: new Date().getTime()
          }
       )
      ];
      const shareData = {
        files: filesArray,
      };
      if (navigator.canShare && navigator.canShare(shareData)) {
          navigator.share(shareData);
      }
  } catch (error) {
    alert(error);
    console.error(error);
  }
}

function _base64ToArrayBuffer(base64) {
    var binary_string = window.atob(base64);
    var len = binary_string.length;
    var bytes = new Uint8Array(len);
    for (var i = 0; i < len; i++) {
        bytes[i] = binary_string.charCodeAt(i);
    }
    return bytes.buffer;
}
//alert("VERSION "+version+" - loaded ");

