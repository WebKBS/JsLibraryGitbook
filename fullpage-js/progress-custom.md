---
description: Progressbar를 커스텀해서 만들기.
---

# Progress Custom

## HTML 예시

```html
<div id="fullpage">
    <div class="section">
        <div class="fp-progress-container"></div>
        <div class="slide">Some section</div>
        <div class="slide">Some section</div>
        <div class="slide">Some section</div>
    </div>
    <div class="section">Some section</div>
    <div class="section">Some section</div>
    <div class="section">Some section</div>
</div>
```

클래스 fp-progress-container는 프로그레스 바가 들어갈 엘리먼트

클래스 slide는 우측 슬라이드.



## CSS 예시

```css

.fp-progress-container {
    position: fixed;
    top: 56px;
    left: 0;
    width: 100%;
    height: 4px;
    z-index: 400;
    display: flex;
    gap: 4px;
}

.fp-progress-bar {
    width: 0%;
    height: 100%;
    background-color: 컬러;
    transition: all 0.2s ease-in;

    &.on {
        flex: 1;
        background-color: 컬러;
    }
}

```

CSS는 디자인에 맞게 자유롭게 설정.

본인은 Progress 바 index를 개별로 주기위해서 flex를 사용해서 gap을 사용했다.

위 HTML fp-progress-container 클래스에 자식요소로 slide index개수 만큼 .fp-progress-bar 클래스 엘리먼트가 추가 될 것이다.



## Javascript

```javascript
window.addEventListener('DOMContentLoaded', () => {

    new fullpage('#fullpage', {
        licenseKey: "라이센스 key!!!!!!!!!!",
        autoScrolling: true,
        scrollHorizontally: true,
        controlArrows: false,
        scrollingSpeed: 300,

        // section 슬라이드가 로드 되고 난 후 이벤트
        afterLoad: function (origin, destination, direction, trigger) {
            let count = destination.item.querySelector('.fp-slidesContainer')?.childElementCount; // 슬라이드 자식요소 갯수, (옵셔널 체이닝)
            let childrenCount = destination.item.querySelector('.fp-progress-container')?.childElementCount; // progress 바 자식요소 갯수 (옵셔널)체이닝

            if (count === 1) { // 슬라이드 갯수가 한개라면 progress 생성 금지
                return;
            }

            if (childrenCount === 0) { // progress 자식엘리먼트 없으면 progress 자식element 생성, 있으면 생성 금지

                for (let i = 0; i < count; i++) {
                    let createEle = document.createElement('div');
                    createEle.className = "fp-progress-bar";
                    destination.item.querySelector('.fp-progress-container').append(createEle);
                    createEle.style.width = (100 / count) + '%';
                }
                destination.item.querySelectorAll('.fp-progress-bar')[0].classList.add('on'); // 컨텐트 로드시 slide 첫번째 index 네비게이션 활성화
            }
        },
        afterSlideLoad: function (section, origin, destination, direction) {
            let progressBar = section.item.querySelectorAll('.fp-progress-bar');

            progressBar.forEach((item, idx) => {
                if (idx === destination.index) {
                    item.classList.add('on'); // 현재 섹션에 대한 네비게이션 항목을 활성화
                } else {
                    item.classList.remove('on'); // 이전 활성화된 항목을 비활성화
                }
            })
        }
    });
})
```



위와 같이 만들면 section의 slide index값에 맞춰 해당  index의progress  바를 활성화 시키는 방법이다.
