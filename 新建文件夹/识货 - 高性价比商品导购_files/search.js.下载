$(function(){
    submitSearch.init();

    if($("#submit_nav").val()!== ""){
        $(".tags a").hide();
        $(".tags .redlink").show();
    }

    $("#submit_nav").focus(function(){
        $(this).attr("placeholder","输入海外商品链接或关键词，点击搜索一键购！");
        $(".tags a").hide();
        $(".tags .redlink").show();
    });

    $("#submit_nav").blur(function(){
        $(this).attr("placeholder","");
        if($(this).val()!== ""){
            $(".tags a").hide();
            $(".tags .redlink").show();
            return false
        }
        $(".tags a").show();
    });
});


var submitSearch = {
    ulist:-1,
    init:function(){
        this.bindFun();
    },
    bindFun:function(){
        var searchSleepSecond = 1000,
            searchStatus = true,
            searchData = new Object(),
            that = this;

        $('#submit_nav').keydown(function(e){
            if(e.keyCode == 38){
                if(that.ulist > 0 && !$(".shihuo-nav-shsug").is(":hidden")){
                    that.ulist--;
                    that.writeFun();
                }
                return false;
            }
            if(e.keyCode == 40 && !$(".shihuo-nav-shsug").is(":hidden")){
                if(that.ulist < $("#shihuo-nav-shsug-ul li").length-1){
                    that.ulist++;
                    that.writeFun();
                }
                return false;
            }
        })

        $('#submit_nav').keyup(function(e){
            if(e.keyCode == 38 || e.keyCode == 40 ){
                return false;
            }

            if(searchStatus && e.keyCode != 13){
                searchStatus = false;
                setTimeout(function(){
                    searchStatus = true;
                } , this.searchSleepSecond);

                var searchContent = $(this).val();
                var searchUrl = '//www.shihuo.cn/api/s';

                if(!searchContent){
                    $(".shihuo-nav-shsug").hide();
                    return false;
                }

                if(typeof searchData[searchContent] == 'undefined'){
                    searchContent = searchContent.replace(/\'/ig,"*");
                    $.ajax({
                       type:'POST',
                       url:searchUrl,
                       dataType:'json',
                       data:{
                           keywords :searchContent
                       },
                       xhrFields:{
                           withCredentials:true
                       },
                       crossDomain: true,
                       success:function (msg) {
                           if(msg.status){
                               searchData[searchContent] = msg.data;
                               var str='';
                               for(var i= 0; i < msg.data.length; i++){
                                   str += '<li class="shsug-overflow" data-key="'+msg.data[i]+'">'+msg.data[i]+'</li>';
                               }

                               $('.shihuo-nav-shsug ul').html(str);
                               $(".shihuo-nav-shsug").show();
                           }
                       },
                        error:function(msg){
                           console.log(msg.statusText)
                        }
                    });
                }else{
                    var str='';
                    for(var i= 0; i < searchData[searchContent].length; i++){
                        str += '<li class="shsug-overflow" data-key="'+searchData[searchContent][i]+'">'+searchData[searchContent][i]+'</li>';
                    }

                    if(str == ''){
                        $(".shihuo-nav-shsug").hide();
                    }else{
                        $(".shihuo-nav-shsug").show();
                        $('.shihuo-nav-shsug ul').html(str);
                    }
                }

               // console.log(searchData);
            }
        });

        $("#shihuo-nav-shsug-ul li").live('mousemove',function(data){
             $(this).addClass('on');
        });

        $("#shihuo-nav-shsug-ul li").live('mouseout',function(data){
             $(this).removeClass('on');
        });
    },
    writeFun:function(){
        $("#shihuo-nav-shsug-ul li").removeClass('on');
        $("#shihuo-nav-shsug-ul li").eq(this.ulist).addClass('on');
        $("#submit_nav").val($("#shihuo-nav-shsug-ul li").eq(this.ulist).html());
    }
}
var second = 0,loadingA,isCancel = false;
function loadingAnimate(){  
    second++;                       
    $(".loadingbar i").animate({width:Math.round(second*20)+"px"},300);                          
}
!(function($){
    var hostname;
    if(window.location.host.indexOf('shihuo.cn') >= 0){
        hostname = 'shihuo'
    }else if(window.location.host.indexOf('haitaodashi.cn') >= 0){
        hostname = 'haitaodashi'
    }

   var url = hostname ==  'haitaodashi' ? '//www.haitaodashi.cn/haitao/daigou' : '//www.shihuo.cn/search';
   var JPlaceHolder = {
      _check : function(){
          return 'placeholder' in document.createElement('input');
      },
      init : function(){
          if(!this._check()){
              this.fix();
          }                          
      },
      fix : function(){
          jQuery('.logos-box input[placeholder]').each(function(index, element) {
              var self = $(this), txt = self.attr('placeholder');
              self.wrap($('<div></div>').css({position:'relative', zoom:'1', border:'none', background:'none', padding:'none', margin:'none'}));                              
              var pos = self.position(), h = self.outerHeight(true)-1, paddingleft = self.css('padding-left');
              var holder = $('<span></span>').text(txt).css({position:'absolute', left:pos.left, top:pos.top, height:h, "line-height":h+"px", paddingLeft:paddingleft, color:'#aaa',display:"none"}).appendTo(self.parent());                              
              $(this).css({"height":h,"line-height":h+"px"});
              self.focusin(function(e) {
                  holder.hide();
              }).focusout(function(e) {
                  if(!self.val()){
                      holder.show();
                  }
              });
              if(!self.val()){
                  holder.show();
              }
              holder.click(function(e) {
                  holder.hide();
                  self.focus();
              });
          });
      }
   };


   var seach_layer = {
     lodingJson:false,
     init:function(){
         this.bindFun();
         this.cancelSearch();
     },
     bindFun:function(){
        var that = this,
        shsug_overflow = $('.shsug-overflow'),
        submit_nav = $("#submit_nav"),
        submit = $("#seach_sub"),
        channel = {
            "sh_basketball":10,
            "sh_running":10,
            "sh_casual":10,
            "sh_youhui":'1,2',
            "sh_haitao":6,
            "sh_tuangou":3,
            "sh_shaiwu":7,
            "sh_shiwu":12
        },
        daceChannel = typeof __daceDataNameOfChannel == "undefined" ? "" : __daceDataNameOfChannel;
        channelType = $.getParameterByName('channelType');

        if(channel[daceChannel]){
            $(".tags a").not(":last-child").each(function(){
                var href = $(this).attr("href")+'&channelType='+channel[daceChannel];
                $(this).attr("href",href);
            });
        }else if(daceChannel == "sh_search" && channelType){
            $(".tags a").not(":last-child").each(function(){
                var href = $(this).attr("href")+'&channelType='+channelType;
                $(this).attr("href",href);
            });
        }
        submit_nav.keyup(function(event){
            var value = submit_nav.val();
            if (event.keyCode == 13 ) {  //判断是否单击的enter按键(回车键)
                generateUrl(value);
            }
        });
        submit.click(function(){
            var value = submit_nav.val();
            generateUrl(value);
        });

         shsug_overflow.live('click',function(){
             var value = $(this).html();
             generateUrl(value);
         });

         function generateUrl(value){
             var seachKeywords = value.replace(/\'/ig,"*");
             if(that.isUrl(value)){
                 that.doyijianGou(value);
                 return false
             }else if(seachKeywords){
                 if(channel[daceChannel]){
                     url= url+'?keywords='+seachKeywords+'&channelType='+channel[daceChannel];
                 }else if(daceChannel == "sh_search" && channelType){
                     url= url+'?keywords='+seachKeywords+'&channelType='+channelType;
                 }else{
                     url= url+'?keywords='+seachKeywords;
                 }
             }
             location.href = url;
             url = '//www.shihuo.cn/search';
         }
     },
     isUrl:function(val){
        var httpReg = new RegExp(/^https?:\/\/(([a-zA-Z0-9_-])+(\.)?)*(:\d+)?(\/((\.)?(\?)?=?&?[a-zA-Z0-9_-](\?)?)*)*$/i);
        var test = httpReg.test(val) || val.indexOf("http") >= 0  || val.indexOf("www.amazon.com") >= 0 || val.indexOf("www.6pm.com") >= 0 ? true : false;
        return test;
     },
     doyijianGou:function(val){
        var that = this;
        if($.trim(val) != "" && $.trim(val) != "输入海外商品链接，点击搜索即可直接通过识货购买"){
              if(that.lodingJson){
                  return false;
               }
               that.lodingJson = true;
              $(".loading,#cancelBtn").fadeIn();
              loadingA = setInterval("loadingAnimate()",500);
              var host = hostname == 'haitaodashi' ? 'www.haitaodashi.cn' : 'www.shihuo.cn';
              $.ajax({
                 type: "POST",
                 url: "//"+host+"/haitao/purchase",
                 data: "url="+encodeURIComponent(val),
                 dataType:"json",
                 success: function(data){
                   clearInterval(loadingA);
                   if(!isCancel){
                      $(".loadingbar i").css("max-width","auto").animate({width:"100%"},300,function(){
                        if(data.status*1 == 0){     
                          location.href = data.data.buy_url;
                          that.lodingJson = false;
                        }else{
                          $(".fade-bg1,.tips-bg1").show();
                          setTimeout(function(){
                             $(".fade-bg1,.tips-bg1").hide();
                             that.lodingJson = false;
                          },3000);
                          second = 0;
                          $(".loadingbar i").css({"max-width":"318px","width":"0px"});
                        }
                        $(".loading,#cancelBtn").hide();
                      });                                              
                    }
                    isCancel = false;                                                                                                                                                                                     
                 }
              });
         }  
     },
     cancelSearch:function(){
        var that = this;
        $("#cancelBtn").click(function(){
           isCancel = true;
           clearInterval(loadingA);     
           that.lodingJson = false;
           second = 0;
           $(".loading,#cancelBtn,.fade-bg1,.tips-bg1").hide();                             
           $(".loadingbar i").css({"max-width":"318px","width":"0px"});
        })
     }                       
   };
   $(function(){
       seach_layer.init();
       JPlaceHolder.init();
   });
})(jQuery);