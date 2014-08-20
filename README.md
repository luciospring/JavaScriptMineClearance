JavaScriptMineClearance
=======================

超简单JS扫雷

<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title></title>
    <script type="text/javascript">
        var landmine = "   ";

        //初始化扫雷游戏主区域界面
        function initMainArea() {
            var innerHtml = ["<table cellpadding = '0' cellspacing='0' border='1' bordercolor='black'>"];
            for (var i = 0; i < 10; i++) {
                innerHtml.push("<tr>");
                for (var j = 0; j < 10; j++) {
                    var id = i + "-" + j;
                    innerHtml.push("<td ><input type='button' value='' id=" + id + " onclick='sweep(this)'></td>");
                }
                innerHtml.push("</tr>");
            }
            innerHtml.push("</table>");
            var mainArea = document.getElementById("MainArea");
            mainArea.innerHTML = innerHtml.join("");

            bulei();
        }


        var dl1, dl2, dl3, dl4;

        //布置地雷
        function bulei() {
            var dl = [];
            do {
                var dilei = Math.round(Math.random() * 99);
                if (dl.indexOf(dl) < 0) {
                    dl.push(dilei);
                }
            } while (dl.length <= 10);

            for (var i = 0; i < dl.length; i++) {
                var index = dl[i];
                var name = parseInt(index / 10) + '-' + index % 10;
                var button = document.getElementById(name);
                button.value = landmine;
            }
        }//end

        //挖雷
        function sweep(button) {
            var btn = button
            if (btn.value == landmine || btn.value == "雷") {
                btn.style.backgroundColor = "red";
                btn.value = "雷";
                alert("你失败了");
            }
            else {
                btn.value = walei(button.id);
                btn.style.backgroundColor = "green";
            }
        }

        //计算周围的累
        function walei(btnID) {
            var dls = 0;//地雷数
            var t = btnID.split("-");
            //var x = Number(t[0]);
            //var y = Number(t[1]);

            var around = getAround(t[0], t[1]);
            dls += Number(getElementyValue(around[0]));
            dls += Number(getElementyValue(around[1]));
            dls += Number(getElementyValue(around[2]));
            dls += Number(getElementyValue(around[3]));
            dls += Number(getElementyValue(around[4]));
            dls += Number(getElementyValue(around[5]));
            dls += Number(getElementyValue(around[6]));
            dls += Number(getElementyValue(around[7]));

			return dls;
            if (dls == 0) {
                for (var i = 0; i < around.length; i++) {
                    walei(around[i]);
                }
            }
            else {
                return dls;

            }
        }

        //通过
        function getElementyValue(elementName) {
            var btn = document.getElementById(elementName);
            if (btn && (btn.value == landmine || btn.value == "雷"))
                return 1;
            else
                return 0;
        }

        //通过元素代号 x1-x2 获得在这个元素周边的8个元素代号，返回长度为8的数组
        function getAround(x1, x2) {
            x1 = Number(x1);
            x2 = Number(x2);
            var v = [getNameByNo(x1 - 1, x2 - 1)
                , getNameByNo(x1 - 1, x2)
                , getNameByNo(x1 - 1, x2 + 1)
                , getNameByNo(x1, x2 - 1)
                , getNameByNo(x1, x2 + 1)
                , getNameByNo(x1 + 1, x2 - 1)
                , getNameByNo(x1 + 1, x2)
                , getNameByNo(x1 + 1, x2 + 1)]
            v = v.filter(function (x) { return x != undefined && x != null; });//需要ECMA5支持
            return v;
        }

        //通过代号得到名字
        function getNameByNo(x1, x2) {
            var result;
            if (x1 > -1 && x2 > -1 && x1 < 10 && x2 < 10)
                result = x1 + '-' + x2;
            return result;
        }
    </script>
    <style type="text/css">
        input {
            text-align: center;
            height: 50px;
            width: 50px;
            font-size: 24px;
            font-weight: bold;
        }
    </style>
</head>
<body onload="initMainArea()" bgcolor="#990099">
    <form name="myform" action="" method="post">
        <h1>超级扫雷</h1>
        <div id="MainArea"></div>
    </form>
</body>
</html>
