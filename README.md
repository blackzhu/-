<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        * {
            padding: 0;
            margin: 0;
            font-size: 17px;
        }

        #email-input {
            width: 100%;
            padding: 5px;
        }

        .wrapper {
            width: 200px;
            margin: 50px auto;
        }

        .email-sug {
            border: 1px solid black;
            background: rgb(243, 241, 241);
            width: 210px;
            overflow: hidden;
            display: none;
        }

        .email-sug>li {
            list-style: none;
            line-height: 25px;
        }

        .email-sug>li:hover {
            list-style: none;
            line-height: 25px;
            background: rgb(207, 206, 206);
            cursor: pointer;
        }

        .keyli {
            background: rgb(207, 206, 206);
        }
    </style>
</head>

<body>
    <div class="wrapper">
        <input id="email-input" type="text">
        <ul id="email-sug-wrapper" class="email-sug"></ul>
    </div>
    <script>
        function diyTrim(str, is_global) {
            return str.replace(/\s+/g, "");
        }
        window.onload = function () {
            var postfixList = ['163.com', 'gmail.com', '126.com', 'qq.com', '263.net', 'zzz.com', 'gma.com'];
            var txt = document.getElementById('email-input');
            var oul = document.getElementById('email-sug-wrapper');
            var li = document.createElement("li");
            var len = postfixList.length;
            var ali = oul.getElementsByTagName('li');
            var front = null;
            var last = null;
            var list = "";

            function show() {//隐藏事件
                oul.style.display = 'block';
            }
            function hide() {//显示事件
                oul.style.display = 'none';
            }
            function stopPropagation(e) {//阻止冒泡
                if (e.stopPropagation) {
                    e.stopPropagation();
                } else {
                    e.cancelBubble = true;
                }
            }

            window.onclick = function () {//点击别处隐藏ul
                oul.style.display = 'none';
            }

            txt.onclick = function (e) {
                if (this.value) {
                    show();
                } else {
                    hide();
                }
                if (list == "") {
                    hide();
                }
                stopPropagation(e);
            }

            function getElementsByClassName(oParent, tagName, className) {//判断对象中的class
                var aEls = oParent.getElementsByTagName(tagName);
                var iLen = aEls.length;
                var num = '';

                for (var i = 0; i < iLen; i++) {
                    var aClass = aEls[i].className.split(' ');//将所有class通过‘空格’拆分，存到数组aClass中，然后循环遍历数组aClass，若存在和目标相同的className，则把包含目标class的标签存到数组arr中，跳出循环。
                    for (var j = 0; j < aClass.length; j++) {
                        if (aClass[j] == className) {
                            num = i;
                            break;//防止一个元素含多个相同的className而被多次选中
                        }
                    }
                }
                return num;
            }

            txt.oninput = function (ev) {
                var oevent = ev || event;
                list = "";

                this.value = diyTrim(this.value);//去空格

                if (this.value.indexOf("@") != -1) {//判断是否输入@
                    var firstget = this.value.indexOf("@");//判断@输入的位置
                    front = this.value.substring(0, firstget);//得到@前的输入内容
                    last = this.value.substring(firstget + 1);//得到@后的输入内容
                    var lastlen = last.length;//@后输入内容的长度

                    for (var p = 0; p < len; p++) {
                        if (last == "") {
                            list += "<li>" + front + '@' + postfixList[p] + "</li>";
                        } else if (last == postfixList[p].slice(0, lastlen)) {
                            list += "<li>" + front + '@' + postfixList[p] + "</li>";
                        }
                    }
                } else {
                    front = this.value;
                    for (var i = 0; i < len; i++) {
                        list += "<li>" + front + '@' + postfixList[i] + "</li>";
                    }
                }
                oul.innerHTML = list;

                if (this.value && list != "") {
                    show();
                    ali[0].classList.add("keyli");
                    for (var i = 0; i < ali.length; i++) {
                        ali[i].onclick = function () {
                            txt.value = this.innerHTML;
                        }
                    };
                    this.onkeydown = function (ev) {
                        var oevent = ev || event;
                        //oevent.preventDefault();//禁止上下左右键对input的操作
                        if (oevent.keyCode == 40) {//向下
                            if (this.value) {
                                var focusli = Number(getElementsByClassName(oul, 'li', 'keyli'));
                                if (focusli >= 0 && focusli < ali.length - 1) {
                                    ali[focusli].classList.remove("keyli");
                                    ali[focusli + 1].classList.add("keyli");
                                }
                                if (focusli == ali.length - 1) {
                                    ali[ali.length - 1].classList.remove("keyli");
                                    ali[0].classList.add("keyli");
                                }
                            }
                        }
                        if (oevent.keyCode == 38) {//向上
                            if (this.value) {
                                var focusli = Number(getElementsByClassName(oul, 'li', 'keyli'));
                                if (focusli > 0 && focusli < ali.length) {
                                    ali[focusli].classList.remove("keyli");
                                    ali[focusli - 1].classList.add("keyli");
                                }
                                if (focusli == 0) {
                                    ali[0].classList.remove("keyli");
                                    ali[ali.length-1].classList.add("keyli");
                                }
                            }
                        }
                        if (oevent.keyCode == 13) {//回车
                            if (this.value) {
                                var focusli = Number(getElementsByClassName(oul, 'li', 'keyli'));
                                txt.value = ali[focusli].innerHTML;
                                hide();
                            }
                        }
                    }
                } else {
                    hide();
                }

                if (list == "") {
                    hide();
                }
            }
        }
    </script>
</body>

</html>
