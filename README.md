// ==UserScript==
// @name         Ship Station Automation
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       Matthew Hamu
// @match        https://ss6.shipstation.com/
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    // Your code here...
    var notSet = true;

    var tag = setInterval(function(){
        if(document.getElementsByClassName("track-PrintPackingSlips").length > 0) {
                 document.getElementsByClassName("track-PrintPackingSlips")[0].addEventListener("click", function() {
                 document.getElementsByClassName("lnk-add-tag")[0].click();
                });
            clearInterval(tag);

            document.getElementById("order-tray").style.visibility = "hidden";
            document.getElementById("topnav").style.visibility = "hidden";
            document.getElementsByClassName("btn-drawer-toggle")[0].style.visibility = "hidden";
        }
    }, 1000);

    setInterval(function () {
    if (document.getElementsByClassName("order-detail").length > 0 && notSet) {
        var btnCount = document.getElementsByClassName("btn-ship").length;

        document.getElementsByClassName("btn-ship")[btnCount - 1].addEventListener("click", function() {
            var i = 0;

            function  checkModalSuccess() {
                setTimeout(function(){
                    if (document.getElementsByClassName("btn-ship").length === (btnCount + 1)) {
                        document.getElementsByClassName("btn-ship")[btnCount].addEventListener("click", function () {
                            function  checkLabelSuccess() {
                                var j = 0;

                                setTimeout(function(){
                                    if (document.getElementsByClassName("btn-ship").length !== (btnCount + 1)) {
                                        document.getElementsByClassName("lnk-scan-barcode")[0].click();
                                    } else if (j >= 120) {
                                        return;
                                    }
                                    else {
                                        j++;
                                        checkLabelSuccess();
                                    }
                                }, 50);
                            }
                            checkLabelSuccess();
                        });
                    } else if (i >= 120) {
                        i++;
                        return;
                    }
                    else {
                        checkModalSuccess();
                    }
                }, 50);
            }
            checkModalSuccess();
        });

        notSet = false;
    } else if (document.getElementsByClassName("order-detail").length === 0) {
        notSet = true;
    }
}, 250);
})();
