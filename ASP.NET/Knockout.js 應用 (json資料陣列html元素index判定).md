目的：建立一個可以根據json幾筆資料，來為html元素欄位(input、div等等)的data-index來定義。

這個在某個資料的.js上，該網頁對應此js，寫在html前端的java也可以，也更容易處理

1.先幫資料class 有一個index
![[Pasted image 20240806104849.png]]
2.設置當前currentIndex，每次依據addItem和removeItemy做+1和-1。
![[Pasted image 20240806105125.png]]
![[Pasted image 20240806105105.png]]
3.json回傳回來可以查看多少筆資料，mappedData的item有index可以使用，detailss是要存取的json，可自行宣告。
![[Pasted image 20240806105530.png]]
4.途中data-bind有value可以透過第一點的資料傳進來使用，attr可以設定data-index欄位抓取index，data-index是自己命名。
![[Pasted image 20240806110911.png]]

5.這個是addItem增加jsonData，toJson是設置好的json變數，3的話就是json用法
![[Pasted image 20240806111458.png]]

6.Detailss的json被宣告在一個ts項目,還有addItem和removeItem的方法，這部分不清楚怎麼透過ts讀取到的。
![[Pasted image 20240806111635.png]]

6.以下方式可以設定每一個json列出來的input值，透過data-index設置每個index的input做對應的處理。

function updateValue() {
    //json的內容
    var viewModel = ko.dataFor($('div[data-bind="visible: detailss().length > 0"]').get(0));

    var inputsE = document.querySelectorAll('[data-index^="InputE"]');
    var inputsME = document.querySelectorAll('[data-index^="InputME"]');
    var inputsDiffE = document.querySelectorAll('[data-index^="InputDiffE"]');

    inputsE.forEach(function (inputE) {
        var index = inputE.getAttribute('data-index').replace('InputE', '');
        index = parseInt(ko.utils.unwrapObservable(index), 10);

        var inputME = document.querySelector('[data-index="InputME' + index + '"]');
        var inputDiffE = document.querySelector('[data-index="InputDiffE' + index + '"]');

        if ((inputE.value || inputME.value) && inputDiffE) {
            var valueE = parseFloat(inputE.value) || 0;
            var valueME = parseFloat(inputME.value) || 0;
            var diffE = Math.abs(valueE - valueME);

            //四捨五入到三位
            $(inputDiffE).val(diffE.toFixed(3));

            //要觸發不然儲存不了
            var event = new Event('input', {
                bubbles: true,
                cancelable: true,
            });
            inputDiffE.dispatchEvent(event);
            $(inputDiffE).trigger('change');
        }
    });

    var inputsN = document.querySelectorAll('[data-index^="InputN"]');
    var inputsMN = document.querySelectorAll('[data-index^="InputMN"]');
    var inputsDiffN = document.querySelectorAll('[data-index^="InputDiffN"]');

    inputsN.forEach(function (inputN) {
        var index = inputN.getAttribute('data-index').replace('InputN', '');
        index = parseInt(ko.utils.unwrapObservable(index), 10);

        var inputMN = document.querySelector('[data-index="InputMN' + index + '"]');
        var inputDiffN = document.querySelector('[data-index="InputDiffN' + index + '"]');

        if ((inputN.value || inputMN.value) && inputDiffN) {
            var valueN = parseFloat(inputN.value) || 0;
            var valueMN = parseFloat(inputMN.value) || 0;
            var diffN = Math.abs(valueN - valueMN);

            //四捨五入到三位
            $(inputDiffN).val(diffN.toFixed(3));

            //要觸發不然儲存不了
            var event = new Event('input', {
                bubbles: true,
                cancelable: true,
            });
            inputDiffN.dispatchEvent(event);
            $(inputDiffN).trigger('change');
        }
    });

    var inputsH = document.querySelectorAll('[data-index^="InputH"]');
    var inputsMH = document.querySelectorAll('[data-index^="InputMH"]');
    var inputsDiffH = document.querySelectorAll('[data-index^="InputDiffH"]');

    inputsH.forEach(function (inputH) {
        var index = inputH.getAttribute('data-index').replace('InputH', '');
        index = parseInt(ko.utils.unwrapObservable(index), 10);
        var inputMH = document.querySelector('[data-index="InputMH' + index + '"]');
        var inputDiffH = document.querySelector('[data-index="InputDiffH' + index + '"]');

        if ((inputH.value || inputMH.value) && inputDiffH) {
            var valueH = parseFloat(inputH.value) || 0;
            var valueMH = parseFloat(inputMH.value) || 0;
            var diffH = Math.abs(valueH - valueMH);

            //四捨五入到三位
            $(inputDiffH).val(diffH.toFixed(3));

            //要觸發不然儲存不了
            var event = new Event('input', {
                bubbles: true,
                cancelable: true,
            });
            inputDiffH.dispatchEvent(event);
            $(inputDiffH).trigger('change');
        }
    });
}