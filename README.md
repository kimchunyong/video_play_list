# video_play_list

## dependency
`handlebars-v4.1.2.js, jquery-3.4.1.min.js`

## 요구사항

- 비디오 플레이 리스트 재생

- 우측 비디오 리스트 데이터만큼 동적생성

- 각 데이터에 해당하는 비디오 리스트 멈추었을시 멈춘 시간 저장후 다시 재생할때 멈춘 부분부터 재생되게 구현

- 비디오재생이 완료되면 완료된상태 체크

- 재생이 끝나면 이전,다음 버튼이 나타나고, 현재 비디오 데이터를 기준으로 이전,다음으로 이동된다.

- 재생이 끝난 뒤에 아무것도 하지않으면 3초뒤에 현재 비디오를 다시 재생한다.

![비디오리스트](https://github.com/kimchunyong/video_play_list/blob/master/ezgif.com-resize.gif)

video_list.js

```javascript
 
//더미 데이터
var getVideoData = function () {
    return new Promise(function (resolve, reject) {
        var videoData = [
            {"idx": 1, "src": "https://vjs.zencdn.net/v/oceans.mp4", "poster": "https://cdn2-www.dogtime.com/assets/uploads/2018/11/dogs-fall-season-pictures-1.jpg", "isCheck": false, "tit": "Introduction", "time": 0},
            {"idx": 2, "src": "https://vjs.zencdn.net/v/oceans.mp4", "poster": "https://s3.amazonaws.com/cdn-origin-etr.akc.org/wp-content/uploads/2017/11/12234558/Chinook-On-White-03.jpg", "isCheck": false, "tit": "Introduction", "time": 0},
            {"idx": 3, "src": "https://vjs.zencdn.net/v/oceans.mp4", "poster": "https://www.rspcansw.org.au/wp-content/uploads/2017/08/50_a-feature_dogs-and-puppies_mobile.jpg", "isCheck": false, "tit": "Introduction", "time": 0},
            {"idx": 4, "src": "https://vjs.zencdn.net/v/oceans.mp4", "poster": "https://cdn-prod.medicalnewstoday.com/content/images/articles/322/322868/golden-retriever-puppy.jpg", "isCheck": false, "tit": "Introduction", "time": 0},
            {"idx": 5, "src": "https://vjs.zencdn.net/v/oceans.mp4", "poster": "https://cdn2-www.dogtime.com/assets/uploads/2018/11/dogs-fall-season-pictures-1.jpg", "isCheck": false, "tit": "Introduction", "time": 0},
            {"idx": 6, "src": "https://vjs.zencdn.net/v/oceans.mp4", "poster": "https://s3.amazonaws.com/cdn-origin-etr.akc.org/wp-content/uploads/2017/11/12234558/Chinook-On-White-03.jpg", "isCheck": false, "tit": "Introduction", "time": 0},
            {"idx": 7, "src": "https://vjs.zencdn.net/v/oceans.mp4", "poster": "https://www.rspcansw.org.au/wp-content/uploads/2017/08/50_a-feature_dogs-and-puppies_mobile.jpg", "isCheck": false, "tit": "Introduction", "time": 0},
            {"idx": 8, "src": "https://vjs.zencdn.net/v/oceans.mp4", "poster": "https://cdn-prod.medicalnewstoday.com/content/images/articles/322/322868/golden-retriever-puppy.jpg", "isCheck": false, "tit": "Introduction", "time": 0},
            {"idx": 9, "src": "https://vjs.zencdn.net/v/oceans.mp4", "poster": "https://cdn2-www.dogtime.com/assets/uploads/2018/11/dogs-fall-season-pictures-1.jpg", "isCheck": false, "tit": "Introduction"},
            {"idx": 10, "src": "https://vjs.zencdn.net/v/oceans.mp4", "poster": "https://s3.amazonaws.com/cdn-origin-etr.akc.org/wp-content/uploads/2017/11/12234558/Chinook-On-White-03.jpg", "isCheck": false, "tit": "Introduction", "time": 0},
            {"idx": 11, "src": "https://vjs.zencdn.net/v/oceans.mp4", "poster": "https://www.rspcansw.org.au/wp-content/uploads/2017/08/50_a-feature_dogs-and-puppies_mobile.jpg", "isCheck": false, "tit": "Introduction", "time": 0},
            {"idx": 12, "src": "https://vjs.zencdn.net/v/oceans.mp4", "poster": "https://cdn-prod.medicalnewstoday.com/content/images/articles/322/322868/golden-retriever-puppy.jpg", "isCheck": false, "tit": "Introduction", "time": 0},
 
        ]
        resolve(videoData);
    })
}
 
getVideoData()
    .then(function (res) {
        var videoArr = res,
            vid = document.querySelector('#my-video'),
            currentNum = 0,
            maxNum = res.length,
            videoListData = {
                text: videoArr,
            };
 
        var player = videojs(vid, {
            inactivityTimeout: 0
        });
 
        //현재 받은 비디오데이터의 맨처음 list 재생준비
        player.poster(videoArr[0].poster);
        player.src(videoArr[0].src);
        $(".video__tit").text(videoArr[0].idx + ". " + videoArr[0].tit);
 
 
        var videoList = new HandleBars(videoListData, '#video__total-template', '.video__menu');
        videoList.render();
 
        var listItem = $('.video__items');
        listItem.eq(0).addClass('active');
        listClick(listItem);
 
        var setSlcect = function (listSelect, currentNum) {
            listSelect.text[currentNum].idx = videoArr[currentNum].idx;
            listSelect.text[currentNum].tit = videoArr[currentNum].tit;
            listSelect.text[currentNum].src = videoArr[currentNum].src;
            listSelect.text[currentNum].poster = videoArr[currentNum].poster;
            listSelect.text[currentNum].isCheck = videoArr[currentNum].isCheck;
        };
 
        player.on('play', function () {
            //동영상 플레이 감지
            removeBtn();
            //console.log(videoListData.text[currentNum].time)
 
            this.currentTime(videoListData.text[currentNum].time);
 
        });
 
        player.on('ended', function () {
            //비디오가 완료되었다면 체크
            videoListData.text[currentNum].isCheck = true;
 
            var videoList = new HandleBars(videoListData, '#video__total-template', '.video__menu');
            videoList.render();
 
            var listItem = $('.video__items');
            listClick(listItem);
 
            $('.video__items').eq(currentNum).addClass('active');
 
            var videoBox = document.querySelector('.vidoe__btn-box');
 
            if (currentNum === 0) {
                videoBox.appendChild(getBtn('nextBtn'));
            } else if (currentNum > 0 && currentNum < maxNum - 1) {
                videoBox.appendChild(getBtn('prevBtn'));
                videoBox.appendChild(getBtn('nextBtn'));
            } else if (currentNum === maxNum - 1) {
                videoBox.appendChild(getBtn('prevBtn'));
            }
 
            prevBtn();
            nextBtn();
 
            (function () {
                //3초뒤에 현재 비디오를 다시 처음부터 재생
                var timer = setTimeout(function () {
                    videoListData.text[currentNum].time = 0;
                    player.play();
                }, 3000);
            })();
 
        });
 
        player.on('pause', function () {
            //현재 비디오 정지 체크
            var isPaused = player.paused();
 
            if (isPaused) {
                //정지 되었다면 현재 멈춘만큼의 시간을 체크해서 저장.
                videoListData.text[currentNum].time = Number(player.cache_.currentTime);
            }
 
            return false;
        })
 
 
 
        function listClick(listItem) {
            listItem.on('click', function (e) {
                try {
                    //클릭시 재생되고있던 비디오 시간 저장
                    var timeCheck = player.currentTime();
                    videoListData.text[currentNum].time = timeCheck;
 
                    var listSelect = {
                        text: videoArr,
                    };
 
                    removeBtn();
 
                    currentNum = $(this).index();
 
                    $('.video__items').removeClass('active');
                    $(this).addClass('active');
 
                    setSlcect(listSelect, currentNum);
 
                    player.poster(listSelect.text[currentNum].poster);
                    player.src(listSelect.text[currentNum].src);
                    $(".video__tit").text(listSelect.text[currentNum].idx + ". " + listSelect.text[currentNum].tit);
 
                    return false;
                } catch (error) {
                    console.warn(error);
                }
            });
        }
 
        function prevBtn() {
            var prevBtn = document.querySelector('.video__prev-btn');
            if (!prevBtn) return;
 
            var listSelect = {
                text: videoArr,
            }
 
            prevBtn.addEventListener('click', function (e) {
                removeBtn();
                $('.video__items').removeClass('active');
                $('.video__items').eq(currentNum - 1).addClass('active');
                currentNum -= 1;
                setSlcect(listSelect, currentNum);
                player.poster(listSelect.text[currentNum].poster);
                player.src(listSelect.text[currentNum].src);
                $(".video__tit").text(listSelect.text[currentNum].idx + ". " + listSelect.text[currentNum].tit);
            });
        }
 
        function nextBtn() {
            var nextBtn = document.querySelector('.video__next-btn');
            if (!nextBtn) return;
 
            var listSelect = {
                text: videoArr,
            }
 
            nextBtn.addEventListener('click', function (e) {
                removeBtn();
                $('.video__items').removeClass('active');
                $('.video__items').eq(currentNum + 1).addClass('active');
                currentNum += 1;
                setSlcect(listSelect, currentNum);
                player.poster(listSelect.text[currentNum].poster);
                player.src(listSelect.text[currentNum].src);
                $(".video__tit").text(listSelect.text[currentNum].idx + ". " + listSelect.text[currentNum].tit);
            });
        }
    });
 
function getBtn(btnName) {
    var videoBtns = document.createElement('button');
 
    videoBtns.textContent = btnName === 'prevBtn' ? '이전' : '다음';
    videoBtns.classList.add(btnName === 'prevBtn' ? 'video__prev-btn' : 'video__next-btn');
 
    return videoBtns;
}
 
function removeBtn() {
    var btnBox = document.querySelector('.vidoe__btn-box');
    var btnBoxLength = btnBox.childNodes.length;
    if (btnBoxLength) {
        Array.from(btnBox.childNodes).map(function (item) {
            btnBox.removeChild(item);
        });
    }
}
 
 
$('.video__nav-close').on('click', function () {
    $('.video__nav')
        .css({
            "position": "absolute",
        })
        .stop()
        .animate({
            "right": "-380px"
        }, 500, function () {
            $('.video__more-btn').removeClass('off');
        });
    $('.video__contents')
        .stop()
        .animate({
            "width": "100%"
        }, 500);
});
 
$('.video__more-btn').on('click', function () {
    $('.video__nav')
        .css({
            "position": "absolute",
        })
        .stop()
        .animate({
            "right": "0"
        }, 500, function () {
            $('.video__more-btn').addClass('off');
        });
    $('.video__contents')
        .stop()
        .animate({
            "width": "80%"
        }, 500);
})

```
html

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<meta http-equiv="X-UA-Compatible" content="ie=edge">
	<title>Document</title>
	<link href="https://vjs.zencdn.net/7.6.0/video-js.css" rel="stylesheet">
	<link rel="stylesheet" href="./css/video.css">
	<script src="./js/handlebars-v4.1.2.js"></script>
	<script src="./js/jquery-3.4.1.min.js"></script>
</head>
<body>
	<!-- edu video s -->
	<div class="video__wrap">
		<div class="video__contents">
			<div class="video__box">
				<p class="video__tit"></p>
				<div class="video__inner-box">
					<video 
						id="my-video" 
						class="video-js vjs-big-play-centered vjs-fluid" 
						controls 
						preload="auto" 
						width="640" 
						height="268" 
						poster="https://vjs.zencdn.net/v/oceans.png"
						data-setup="{'techOrder':['html5','flash','flvjs']}">
						<source src="https://vjs.zencdn.net/v/oceans.mp4" type="video/mp4">
					</video>
				</div>
				<div class="vidoe__btn-box"></div>
			</div>
		</div>
		<div class="video__nav">
			<div class="video__nav-header">
				<div class="video__nav-inner">
					<!-- video__nav-top s -->
					<div class="video__nav-top">
						<h2 class="video__nav-tit">강좌 콘텐츠</h2>
						<span class="video__nav-close">X</span>
					</div>
					<!-- video__nav-top e -->
					<!-- video__menu s -->
					<ul class="video__menu">
	
					</ul>
					<!-- video__menu e -->
				</div>
			</div>
			<button class="video__more-btn off" title="더보기" alt="강좌 컨텐츠 더보기">+</button>
		</div>
	</div>
	<!-- edu video e -->
	<script id="video__total-template" type="text/x-handlebars-template">
			{{#text}}
			{{#if isCheck}}
			<li class="video__items complate" data-idx={{idx}}>
				<span class="video__items-tit">{{idx}}. {{tit}}</span>
				<span class="video__player-icon">
					<svg width="18" aria-hidden="true" data-prefix="fal" data-icon="play-circle" class="svg-inline--fa fa-play-circle fa-w-16" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path fill="currentColor" d="M256 504c137 0 248-111 248-248S393 8 256 8 8 119 8 256s111 248 248 248zM40 256c0-118.7 96.1-216 216-216 118.7 0 216 96.1 216 216 0 118.7-96.1 216-216 216-118.7 0-216-96.1-216-216zm331.7-18l-176-107c-15.8-8.8-35.7 2.5-35.7 21v208c0 18.4 19.8 29.8 35.7 21l176-101c16.4-9.1 16.4-32.8 0-42zM192 335.8V176.9c0-4.7 5.1-7.6 9.1-5.1l134.5 81.7c3.9 2.4 3.8 8.1-.1 10.3L201 341c-4 2.3-9-.6-9-5.2z"></path>
					</svg>
					<span class="video__player-time">02: 00</span>
				</span>
			</li>
			{{else}}
			<li class="video__items" data-idx={{idx}}>
				<span class="video__items-tit">{{idx}}. {{tit}}</span>
				<span class="video__player-icon">
					<svg width="18" aria-hidden="true" data-prefix="fal" data-icon="play-circle" class="svg-inline--fa fa-play-circle fa-w-16" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path fill="currentColor" d="M256 504c137 0 248-111 248-248S393 8 256 8 8 119 8 256s111 248 248 248zM40 256c0-118.7 96.1-216 216-216 118.7 0 216 96.1 216 216 0 118.7-96.1 216-216 216-118.7 0-216-96.1-216-216zm331.7-18l-176-107c-15.8-8.8-35.7 2.5-35.7 21v208c0 18.4 19.8 29.8 35.7 21l176-101c16.4-9.1 16.4-32.8 0-42zM192 335.8V176.9c0-4.7 5.1-7.6 9.1-5.1l134.5 81.7c3.9 2.4 3.8 8.1-.1 10.3L201 341c-4 2.3-9-.6-9-5.2z"></path>
					</svg>
					<span class="video__player-time">02: 00</span>
				</span>
			</li>
			{{/if}}
			{{/text}}
		</script>
//handlebar.js에 핸들바를 커스터마이징해서 빠르게 사용가능하도록 구현해놓은건 현재 주제와 무관해서 올리지않은상태입니다.
		<script src="./js/handlebar.js"></script>
		<!-- If you'd like to support IE8 (for Video.js versions prior to v7) -->
		<script src="https://vjs.zencdn.net/ie8/1.1.2/videojs-ie8.min.js"></script>
		<script src='https://vjs.zencdn.net/7.6.0/video.js'></script>
		<script src="./js/video_list.js"></script>
</body>
</html>
```

