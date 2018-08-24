<template>
  <div class="hello">
    <!-- <div class="tool" id="tool_pdfobject">
      <h2>PDFObject</h2>
      <div id="pdfobject"></div>  
    </div> -->
    <!-- <div class="tool" id="tool_pdfjs">
      <h2>PDF.js</h2>
      <div id="viewerContainer">
        <div id="viewer" class="pdfViewer"></div>
      </div>
    </div> -->
    <!-- <div class="tool" id="tool_viewerjs">
      <iframe src = "/ViewerJS/#../static/sample3.pdf" width='800' height='800' allowfullscreen webkitallowfullscreen></iframe>
    </div> -->
    <div class="tool" id="tool_pdfsvg">
      <div id="svgContainer">
        <div id="viewer" class="pdfViewer"></div>
      </div>
    </div>
    <iframe src="https://nuwildcat-my.sharepoint.com/personal/djn093_ads_northwestern_edu/_layouts/15/Doc.aspx?sourcedoc={2eba5705-895b-4830-a163-1a5aa870a482}&amp;action=embedview&amp;wdAr=1.7777777777777777" width="350px" height="221px" frameborder="0">This is an embedded <a target="_blank" href="https://office.com">Microsoft Office</a> presentation, powered by <a target="_blank" href="https://office.com/webapps">Office Online</a>.</iframe>
  </div>
</template>

<script>


var l = console.log.bind(console);


import * as PDFObject from '../../PDFObject-master/pdfobject.js'
import * as pdfjsLib from 'pdfjs-dist';
import * as pdfjsViewer from 'pdfjs-dist/web/pdf_viewer'
// import * as ViewerJS from '../../ViewerJS/'

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  methods: {
    pdfObjectMount: function() {
      var options = {
        height: "800px",
        pdfOpenParams: { view: 'FitV', page: '2' }
      };
      
      PDFObject.embed("/static/sample2.pdf", "#pdfobject", options);
    },
    pdfjsMount: function() {
      if (!pdfjsLib.getDocument || !pdfjsViewer.PDFViewer)  { alert('Please build the pdfjs-dist library using\n' + '  `gulp dist-install`'); }
      pdfjsLib.GlobalWorkerOptions.workerSrc = '/node_modules/pdfjs-dist/build/pdf.worker.js';
      var CMAP_URL = '/node_modules/pdfjs-dist/cmaps/';
      var CMAP_PACKED = true;

      var DEFAULT_URL = '/static/sample2.pdf';
      var SEARCH_FOR = ''; // try 'Mozilla';

      var container = document.getElementById('viewerContainer');

      // (Optionally) enable hyperlinks within PDF files.
      var pdfLinkService = new pdfjsViewer.PDFLinkService();

      var pdfViewer = new pdfjsViewer.PDFViewer({
        container: container,
        linkService: pdfLinkService,
      });
      pdfLinkService.setViewer(pdfViewer);

      // (Optionally) enable find controller.
      var pdfFindController = new pdfjsViewer.PDFFindController({
        pdfViewer: pdfViewer,
      });
      pdfViewer.setFindController(pdfFindController);

      container.addEventListener('pagesinit', function () {
        // We can use pdfViewer now, e.g. let's change default scale.
        pdfViewer.currentScaleValue = 'page-width';

        if (SEARCH_FOR) { // We can try search for things
          pdfFindController.executeCommand('find', {query: SEARCH_FOR});
        }
      });

      // Loading document.
      pdfjsLib.getDocument({
        url: DEFAULT_URL,
        cMapUrl: CMAP_URL,
        cMapPacked: CMAP_PACKED,
      }).then(function(pdfDocument) {
        // Document loaded, specifying document for the viewer and
        // the (optional) linkService.
        pdfViewer.setDocument(pdfDocument);

        pdfLinkService.setDocument(pdfDocument, null);
      });
    },
    svgjsMount: function() {
      if (!pdfjsLib.getDocument || !pdfjsViewer.PDFViewer)  {
        alert('Please build the pdfjs-dist library using\n' +
              '  `gulp dist-install`');
      }

      // The workerSrc property shall be specified.
      //
      pdfjsLib.GlobalWorkerOptions.workerSrc =
        '../../node_modules/pdfjs-dist/build/pdf.worker.js';

      // Some PDFs need external cmaps.
      //
      var CMAP_URL = '../../node_modules/pdfjs-dist/cmaps/';
      var CMAP_PACKED = true;

      var DEFAULT_URL = 'http://localhost:8082/sample-link_1.pdf';
      var SEARCH_FOR = ''; // try 'Mozilla';

      var container = document.getElementById('svgContainer');

      // (Optionally) enable hyperlinks within PDF files.
      var pdfLinkService = new pdfjsViewer.PDFLinkService();

      var pdfViewer = new pdfjsViewer.PDFViewer({
        container: container,
        linkService: pdfLinkService,
        renderer: 'svg',
        textLayerMode: 0,
      });
      pdfLinkService.setViewer(pdfViewer);

      // (Optionally) enable find controller.
      var pdfFindController = new pdfjsViewer.PDFFindController({
        pdfViewer: pdfViewer,
      });
      pdfViewer.setFindController(pdfFindController);

      container.addEventListener('pagesinit', function () {
        // We can use pdfViewer now, e.g. let's change default scale.
        pdfViewer.currentScaleValue = '1';

        if (SEARCH_FOR) { // We can try search for things
          pdfFindController.executeCommand('find', {query: SEARCH_FOR});
        }
      });

      // Loading document.
      pdfjsLib.getDocument({
        url: DEFAULT_URL,
        cMapUrl: CMAP_URL,
        cMapPacked: CMAP_PACKED,
      }).then(function(pdfDocument) {
        // Document loaded, specifying document for the viewer and
        // the (optional) linkService.
        pdfViewer.setDocument(pdfDocument);
        pdfLinkService.setDocument(pdfDocument, null);
      });
    }
  },

  mounted() {
    // this.pdfObjectMount()
    // this.pdfjsMount()
    this.svgjsMount()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.tool{
  border: 1px solid red;
  padding: 10px;
  margin: 30px;
}


</style>
