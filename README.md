# AI_project1

본 프로그램은 **여행**을 주제로 여러 프로그램을 소개하는 홈페이지입니다.

현재 게시된 프로그램 목록<br><br>
1. 추천 여행지 찾기(얼굴 인공지능 분석을 통해 여행지 추천 받기)<br>
___
- 각국에서 선호하는 유명인과 나의 얼굴을 인공지능을 통해 비교해 여행지 및 닮은 유명인을 찾아줍니다. 현재 총 6개의 국가(한국, 미국, 영국, 일본, 캐나다, 태국)의 데이터를 활용해 결과를 제공합니다.
___
입력된 이미지는 Teachable machine의 인공지능 모델로 보내진다. 이 후 예상 확률을 기준으로 국가를 출력한다.
```js
         const URL = "https://teachablemachine.withgoogle.com/models/op95WYIQL/";

        let model, webcam, labelContainer, maxPredictions, google_travel;

        // Load the image model and setup the webcam
        async function init() {
            const modelURL = URL + "model.json";
            const metadataURL = URL + "metadata.json";

            // load the model and metadata
            // Refer to tmImage.loadFromFiles() in the API to support files from a file picker
            // or files from your local hard drive
            // Note: the pose library adds "tmImage" object to your window (window.tmImage)
            model = await tmImage.load(modelURL, metadataURL);
            maxPredictions = model.getTotalClasses();

            // append elements to the DOM
            labelContainer = document.getElementById("label-container");
            for (let i = 0; i < maxPredictions; i++) { // and class labels
                labelContainer.appendChild(document.createElement("div"));
            }
        }
```
만일 예상한 값들의 0번 인덱스가 한국일 때(에측 확률이 가장 높은 국가가 한국일 때 ) predict_korea()함수를 실행하고 resultmassage를 출력한다.
```js
  var resultmassage ;
            if (prediction[0].className == "korea") {
                init_korea().then(function () {
                    predict_korea();
                });
                resultmassage = "한국"
                examplemassage = "저의 추천 여행지는 <strong>한국</strong>입니다.<br> 한국의 <strong>제주도</strong>, <strong>남해 다랭이마을</strong> 등으로 한국여행을 떠나보는 것은 어떨까요?<br>한국에서 <strong>가장 닮은 유명인</strong>은 바로!!!"
                
            }
```
predict_korea() 함수는 한국인들의 이미지가 학습되어 있는 Teachable machine 모델로 이동하고 예측 확률이 높은 레이블의 이름의 구글 링크와 결과 이미지를 출력한다.
```js
    <script type="text/javascript">
        //한국 모델 삽입
        const URL_korea = "https://teachablemachine.withgoogle.com/models/ZQIbYrUzv/";
        let model_korea, labelContainer_korea, maxPredictions_korea, page_value;


        // Load the image model and setup the webcam
        async function init_korea() {
            const modelURL_korea = URL_korea + "model.json";
            const metadataURL_korea = URL_korea + "metadata.json";

            // load the model and metadata
            // Refer to tmImage.loadFromFiles() in the API to support files from a file picker
            // or files from your local hard drive
            // Note: the pose library adds "tmImage" object to your window (window.tmImage)
            model_korea = await tmImage.load(modelURL_korea, metadataURL_korea);
            maxPredictions_korea = model_korea.getTotalClasses();

            // append elements to the DOM
            labelContainer_korea = document.getElementById("label-container");
            for (let i = 0; i < maxPredictions_korea; i++) { // and class labels
                labelContainer_korea.appendChild(document.createElement("div"));
            }
        }


        // run the webcam image through the image model
        async function predict_korea() {
            // predict can take in an image, video or canvas html element
            var image = document.getElementById("face-image")
            const prediction_korea = await model_korea.predict(image, false);
            prediction_korea.sort((a, b) => parseFloat(b.probability) - parseFloat(a.probability));
            $('#loading').hide();
            for (let i = 0; i < 1; i++) {
                const classPrediction_korea =
                    prediction_korea[i].className;
                labelContainer_korea.childNodes[i].innerHTML = classPrediction_korea;
            }
            page_value = prediction_korea[0].className
            result_image()
            $('#result-image').show();


        }
        function gopage() {
            window.open = "https://www.google.com/search?q=" + page_value;
        }

        function result_image() {
            document.getElementById("result-image").src = "./result_img_man/" + page_value + ".jpg";
        }



    </script>
```

