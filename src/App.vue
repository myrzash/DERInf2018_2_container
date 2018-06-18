<template>
    <div>
        <div class="bg" id="background"></div>
        <div class="fg-resizable">
            <iframe src="der/index.html" frameborder="0" id="frame" seamless='seamless' scrolling='no'
                    allowtransparency='true'></iframe>
        </div>
    </div>
</template>

<script>
    var scaleCoefficient = 1;
    const relW = 1600, relH = 1200;

    export default {
        data: () => ({}),
        mounted() {
            let lang = 'ru';
            let langHref = location.href.toString();
            let start = langHref.indexOf("index.html?");
            if (start !== -1) {
                start += "index.html?".length;
                langHref = langHref.substr(start, start + 2);
                if (['kz', 'en', 'ru'].includes(langHref)) lang = langHref
            }

            let frame = document.getElementById('frame');
            frame.setAttribute('src','der/index.html#/'+lang);

            let updateSize = this.updateSize;
            updateSize()
            window.addEventListener('resize', function () {
                updateSize()
            });
        },
        methods: {
            updateSize() {
                let fgResizable = document.querySelector('.fg-resizable');
                let w = window.innerWidth;
                let h = window.innerHeight;

                if (w < relW || h < relH) {
                    if (h / w >= relH / relW) {
                        scaleCoefficient = w / relW;
                    } else {
                        scaleCoefficient = h / relH;
                    }
                    scaleCoefficient = scaleCoefficient >= 0.97 ? 1 : scaleCoefficient
                    fgResizable.style.transform = "scale3d(" + scaleCoefficient + ", " + scaleCoefficient + ", " + scaleCoefficient + ")";
                }
            }
        }
    }
</script>

<style>

    body {
        background: #333;
    }

    .fg-resizable {
        position: fixed;
        z-index: 2;
        width: 1600px;
        height: 1200px;
        background-size: 1600px auto;
        top: 50%;
        left: 50%;
        margin-top: -600px;
        margin-left: -800px;
        overflow: hidden;
    }

    #frame {
        width: 1600px;
        height: 1200px;
        background-color: transparent;
    }

    #loader {
        position: fixed;
        width: 280px;
        height:400px;
        margin-top: -200px;
        margin-left: -140px;
        left: 50%;
        top: 50%;
    }
</style>
