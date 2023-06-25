---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

title: "Home"
layout: default
group: "navigation"
---
<style>
#unity-container { position: static }
#unity-container.unity-desktop { margin: auto; position: static; width: 100%; height: 100% }
#unity-container.unity-mobile { width: 100%; height: 100% }
#unity-canvas { background: #231F20 }
.unity-mobile #unity-canvas { width: 100%; height: 100% }
#unity-loading-bar { position: absolute; left: 50%; top: 50%; transform: translate(-50%, -50%); display: none }
#unity-logo { width: 154px; height: 130px; background: url('assets/unity-logo-dark.png') no-repeat center }
#unity-progress-bar-empty { width: 141px; height: 18px; margin-top: 10px; background: url('assets/progress-bar-empty-dark.png') no-repeat center }
#unity-progress-bar-full { width: 0%; height: 18px; margin-top: 10px; background: url('assets/progress-bar-full-dark.png') no-repeat center }
#unity-footer { position: relative }
.unity-mobile #unity-footer { display: none }
#unity-webgl-logo { float:left; width: 204px; height: 38px; background: url('assets/webgl-logo.png') no-repeat center }
#unity-build-title { float: right; margin-right: 10px; line-height: 38px; font-family: arial; font-size: 18px }
#unity-fullscreen-button { float: right; width: 38px; height: 38px; background: url('assets/fullscreen-button.png') no-repeat center }
#unity-warning { position: absolute; left: 50%; top: 5%; transform: translate(-50%); background: white; padding: 10px; display: none }
</style>
<div id="unity-container" class="unity-desktop">
  <canvas id="unity-canvas" width=960 height=600></canvas>
  <div id="unity-loading-bar">
    <div id="unity-logo"></div>
    <div id="unity-progress-bar-empty">
      <div id="unity-progress-bar-full"></div>
    </div>
  </div>
  <div id="unity-warning"> </div>
  <div id="unity-footer">
    <div id="unity-fullscreen-button"></div>
  </div>
</div>
<script>
  var container = document.querySelector("#unity-container");
  var canvas = document.querySelector("#unity-canvas");
  var loadingBar = document.querySelector("#unity-loading-bar");
  var progressBarFull = document.querySelector("#unity-progress-bar-full");
  var fullscreenButton = document.querySelector("#unity-fullscreen-button");
  var warningBanner = document.querySelector("#unity-warning");

  // Shows a temporary message banner/ribbon for a few seconds, or
  // a permanent error message on top of the canvas if type=='error'.
  // If type=='warning', a yellow highlight color is used.
  // Modify or remove this function to customize the visually presented
  // way that non-critical warnings and error messages are presented to the
  // user.
  function unityShowBanner(msg, type) {
    function updateBannerVisibility() {
      warningBanner.style.display = warningBanner.children.length ? 'block' : 'none';
    }
    var div = document.createElement('div');
    div.innerHTML = msg;
    warningBanner.appendChild(div);
    if (type == 'error') div.style = 'background: red; padding: 10px;';
    else {
      if (type == 'warning') div.style = 'background: yellow; padding: 10px;';
      setTimeout(function() {
        warningBanner.removeChild(div);
        updateBannerVisibility();
      }, 5000);
    }
    updateBannerVisibility();
  }

  var buildUrl = "Build";
  var loaderUrl = buildUrl + "/build-uncompressed.loader.js";
  var config = {
    dataUrl: buildUrl + "/build-uncompressed.data",
    frameworkUrl: buildUrl + "/build-uncompressed.framework.js",
    codeUrl: buildUrl + "/build-uncompressed.wasm",
    streamingAssetsUrl: "StreamingAssets",
    companyName: "DefaultCompany",
    productName: "HORSE PLINKO",
    productVersion: "0.1",
    showBanner: unityShowBanner,
  };

  // By default Unity keeps WebGL canvas render target size matched with
  // the DOM size of the canvas element (scaled by window.devicePixelRatio)
  // Set this to false if you want to decouple this synchronization from
  // happening inside the engine, and you would instead like to size up
  // the canvas DOM size and WebGL render target sizes yourself.
  // config.matchWebGLToCanvasSize = false;

  console.log("made it here!");
  if (/iPhone|iPad|iPod|Android/i.test(navigator.userAgent)) {
    console.log("you're on mobile!");
    container.className = "unity-mobile";
    // Avoid draining fillrate performance on mobile devices,
    // and default/override low DPI mode on mobile browsers.
    config.devicePixelRatio = 1;
    unityShowBanner('WebGL builds are not supported on mobile devices.');
  } else {
    canvas.style.width = "960px";
    canvas.style.height = "600px";
  }
  loadingBar.style.display = "block";

  var script = document.createElement("script");
  script.src = loaderUrl;
  script.onload = () => {
    createUnityInstance(canvas, config, (progress) => {
      progressBarFull.style.width = 100 * progress + "%";
    }).then((unityInstance) => {
      loadingBar.style.display = "none";
      fullscreenButton.onclick = () => {
        unityInstance.SetFullscreen(1);
      };
    }).catch((message) => {
      alert(message);
    });
  };
  document.body.appendChild(script);
</script>
