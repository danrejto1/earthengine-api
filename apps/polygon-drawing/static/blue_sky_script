
// ClearPolygon()
// CancelPolygon()
// handleNewPolygon(...)
// UpdateSelectedMapOverlay()

const AppTitle = 'Blue Sky Albedo';
const AppTitleOldFashion = 'BlueSkyAlbedo';
const KML_MIME_TYPE = 'application/vnd.google-earth.kml';

var MapIDInfoDataRequestTimeOut = 20; // timeout in seconds.
var PolygonDataRequestTimeOut = 50; // timeout in seconds.
var PolygonDownloadRequestTimeOut = 50; // timeout in seconds.
var LeftWrapperMinWidth = 200;
var RightWrapperMinWidth = 200;
var ColorRampMaxWidth = 600;

var General_Tab_ID = 0;
var Options_Tab_ID = 1;
var Data_Tab_ID = 2;
var Charts_Tab_ID = 3;

var LastTimeStampForMapID = -1;
var LastTimeStampForPoygonData = -1;
var LastTimeStampForPoygonDownload = -1;
var LastTimeStampForPoygonChartData = -1;
var CurrentOverlayID = '';
var CurrentOverlayName = '';

var drawingManager;


var map;
var currentPolygon;

var DataChart_YaxisValues = [];
var DataChart_XaxisValues = [];
var DataChart_Title = '';


var AlertName_SelectedRegionDownloadRequest = 'PolygonDownloadRequest';
var AlertName_SelectedRegionDataRequest = 'PolygonDataRequest';
var AlertName_SelectedRegionChartDataRequest = 'PolygonChartDataRequest';

var LastColorRampMinValue = 0;
var LastColorRampMaxValue = 0;
var LastColorRampPaletteValue = '';


var OverlayDataMinValue = [];
var OverlayDataMaxValue = [];
var OverlayDataPaletteValue = [];


OverlayDataMinValue[0] = 0; 		OverlayDataMaxValue[0] = 0; 		OverlayDataPaletteValue[0] = ''; 
OverlayDataMinValue[1] = -0.002; 	OverlayDataMaxValue[1] = 0.002; 	OverlayDataPaletteValue[1] = '0000FF,00FFFF,FFFF00,FF0000'; 
OverlayDataMinValue[2] = 0; 		OverlayDataMaxValue[2] = 1; 		OverlayDataPaletteValue[2] = '0000FF,00FFFF,FFFF00,FF0000'; 
OverlayDataMinValue[3] = 0; 		OverlayDataMaxValue[3] = 1; 		OverlayDataPaletteValue[3] = '0000FF,00FFFF,FFFF00,FF0000'; 
OverlayDataMinValue[4] = 0; 		OverlayDataMaxValue[4] = 1; 		OverlayDataPaletteValue[4] = '0000FF,00FFFF,FFFF00,FF0000'; 
OverlayDataMinValue[5] = 0; 		OverlayDataMaxValue[5] = 1; 		OverlayDataPaletteValue[5] = '0000FF,00FFFF,FFFF00,FF0000'; 
OverlayDataMinValue[6] = 0; 		OverlayDataMaxValue[6] = 1; 		OverlayDataPaletteValue[6] = '0000FF,00FFFF,FFFF00,FF0000'; 
OverlayDataMinValue[7] = 0; 		OverlayDataMaxValue[7] = 1; 		OverlayDataPaletteValue[7] = '0000FF,00FFFF,FFFF00,FF0000'; 
OverlayDataMinValue[8] = 0; 		OverlayDataMaxValue[8] = 1; 		OverlayDataPaletteValue[8] = '0000FF,00FFFF,FFFF00,FF0000'; 
OverlayDataMinValue[9] = 0; 		OverlayDataMaxValue[9] = 1; 		OverlayDataPaletteValue[9] = '0000FF,00FFFF,FFFF00,FF0000'; 
OverlayDataMinValue[10] = 0; 		OverlayDataMaxValue[10] = 1; 		OverlayDataPaletteValue[10] = '0000FF,00FFFF,FFFF00,FF0000'; 
OverlayDataMinValue[11] = 0; 		OverlayDataMaxValue[11] = 1; 		OverlayDataPaletteValue[11] = '0000FF,00FFFF,FFFF00,FF0000'; 
OverlayDataMinValue[12] = 0; 		OverlayDataMaxValue[12] = 1; 		OverlayDataPaletteValue[12] = '0000FF,00FFFF,FFFF00,FF0000'; 
OverlayDataMinValue[13] = 0; 		OverlayDataMaxValue[13] = 1; 		OverlayDataPaletteValue[13] = '0000FF,00FFFF,FFFF00,FF0000'; 
OverlayDataMinValue[14] = 0; 		OverlayDataMaxValue[14] = 1; 		OverlayDataPaletteValue[14] = '0000FF,00FFFF,FFFF00,FF0000'; 
OverlayDataMinValue[15] = 0; 		OverlayDataMaxValue[15] = 1; 		OverlayDataPaletteValue[15] = '0000FF,00FFFF,FFFF00,FF0000'; 
OverlayDataMinValue[16] = 0; 		OverlayDataMaxValue[16] = 1; 		OverlayDataPaletteValue[16] = '0000FF,00FFFF,FFFF00,FF0000'; 
OverlayDataMinValue[17] = 0; 		OverlayDataMaxValue[17] = 1; 		OverlayDataPaletteValue[17] = '0000FF,00FFFF,FFFF00,FF0000'; 



	function initializeApp() {
		
      	google.charts.load("current", {packages:["corechart"]});
		ResizeMapDivs(jQuery(window).width(), jQuery(window).height());
		initializeMap();
		UpdateSelectedMapOverlay();
		
	}


	// Initialize the Google Map.
	function initializeMap() {

		var myLatLng = new google.maps.LatLng(49.397, 22.044);
		var mapOptions = {
			center: myLatLng,
			zoom: 4,
			maxZoom: 12,
			streetViewControl: false
		};

 		// Create the base Google Map.
  		map = new google.maps.Map(document.getElementById('map'), mapOptions);

		drawingManager = createDrawingManager(map);

		initRegionPicker();

	}




	function AddNewMapType(mapId, token) {

    	var baseUrl = 'https://earthengine.googleapis.com/map';
		var name = 'imageOverlayTiles';
    	var overlay = new ee.MapLayerOverlay(baseUrl, mapId, token, {name: name});
	
    	overlay.addTileCallback((function(event) {
  			setAlert(name, 'info', event.count + ' tiles remaining.');

      		if (event.count === 0) {
        		removeAlert(name);
      		}
    	}));


  		// Add the EE layer to the map.
 		 map.overlayMapTypes.setAt(0 , overlay);

	}


	// *******************************************************************************************
	// ********************************* Selected Region Polygon *********************************

	/**
	 * Creates a drawing manager for the passed-in map.
	 * @param {google.maps.Map} map The map for which to create a drawing
	 *     manager.
	 * @return {google.maps.drawing.DrawingManager} A drawing manager for
	 *     the given map.
	 */
	function createDrawingManager(map) {
		var drawingManager = new google.maps.drawing.DrawingManager({
			drawingControl: false,
			polygonOptions: {
				fillColor: '#ff0000',
				strokeColor: '#ff0000'
			}
		});
		drawingManager.setMap(map);
		return drawingManager;
	}




	/** 
	 * Initializes the region picker.
	 */
	function initRegionPicker() {
		
		// Respond when the user chooses to draw a polygon.
		jQuery('.region #draw-selected-region-button').click(AllowDrawPolygon);

		// Respond when the user draws a polygon on the map.
		google.maps.event.addListener(
			drawingManager, 'overlaycomplete',
				(function(event) {
					if (getDrawingModeEnabled()) {
						handleNewPolygon(event.overlay);
					} else {
						event.overlay.setMap(null);
					}
		}));

		// Cancel drawing mode if the user presses escape.
		jQuery(document).keydown((function(event) {
			if (event.which == 27) CancelPolygon();
		}));

		// Respond when the user selects to start a polygon drawing.
		jQuery('.region #draw-selected-region-button').click(AllowDrawPolygon);

		// Respond when the user selects to load a saved polygon region.
		jQuery('.region #load-selected-region-button').click(LoadPolygon);

		// Respond when the user cancels region drawing.
		jQuery('.region #cancel-selected-region-button').click(CancelPolygon);

		// Respond when the user clears the region.
		jQuery('.region #clear-selected-region-button').click(ClearPolygon);
  
		// Respond when the user focus on the region.
		jQuery('.region #focus-selected-region-button').click(FocusOnPolygon);

		// Respond when the user savess the region.
		jQuery('.region #save-selected-region-button').click(SavePolygon);

		// Respond when the user selects to retrieve data of the selected region.
		jQuery('#selected-region-data-panel #request-data-button').click(requestDataForSelectedRegionMean);
		// Respond when the user selects to retrieve median value of the selected region.
		jQuery('#selected-region-data-panel #request-data-median-value-button').click(requestDataForSelectedRegionMedian);
		// Respond when the user selects to retrieve max value of the selected region.
		jQuery('#selected-region-data-panel #request-data-max-value-button').click(requestDataForSelectedRegionMax);
		// Respond when the user selects to retrieve min value of the selected region.
		jQuery('#selected-region-data-panel #request-data-min-value-button').click(requestDataForSelectedRegionMin);

		// Respond when the user selects to download the selected region.
		jQuery('#selected-region-data-panel #request-download-url-button').click(requestDownloadForSelectedRegion);

		// Respond when the user selects to retrieve chart data about the selected region.
		jQuery('#selected-region-chart-panel #request-chart-data-button').click(requestChartDataForSelectedRegion);

		// Respond when the user selects to refresh the chart about the selected region.
		jQuery('#selected-region-chart-panel #refresh-chart-data-button').click(refreshChartForSelectedRegion);
	}


function requestDataForSelectedRegionMean () {	
	requestDataForSelectedRegion('mean', 'Mean value:', 'mean value')
}
function requestDataForSelectedRegionMedian () {	
	requestDataForSelectedRegion('median', 'Median value:', 'median value')
}
function requestDataForSelectedRegionMax () {	
	requestDataForSelectedRegion('max', 'Maximum value:', 'max value')
}
function requestDataForSelectedRegionMin () {	
	requestDataForSelectedRegion('min', 'Minimum value:', 'min value')
}
function refreshChartForSelectedRegion(){
	CreateChart()
}


	/** 
	 * Initializes the region polygon drawing fro the user to draw the polygon.
	 */
	function AllowDrawPolygon() {
		setDrawingModeEnabled(true);
	}

	/** 
	 * Cancels the region polygon drawing.
	 */
	function CancelPolygon() {
		setDrawingModeEnabled(false);
	}



	/**
	 * Returns the coordinates of the currently drawn polygon.
	 * @return {Array<Array<number>>} A list of coordinates describing
	 *     the currently drawn polygon (or null if no polygon is drawn).
	 */
	function getPolygonCoordinates() {
		var points = currentPolygon.getPath().getArray();
		var twoDimensionalArray = points.map(function(point) {
			return [point.lng(), point.lat()];
		});
		return twoDimensionalArray;
	}


	/**
	 * Sets whether drawing on the map is enabled.
	 * @param {boolean} enabled Whether drawing mode is enabled.
	 */
	function setDrawingModeEnabled(enabled) {
		jQuery('.region').toggleClass('drawing', enabled);
		var mode = enabled ? google.maps.drawing.OverlayType.POLYGON : null;
		drawingManager.setOptions({drawingMode: mode});
	}


	/**
	 * Gets whether drawing on the map is enabled.
	 * @return {boolean} Whether drawing mode is enabled.
	 */
	function getDrawingModeEnabled() {
		return jQuery('.region').hasClass('drawing');
	}


	/**
	 * Clears the current polygon from the map and enables drawing. 
	 */
	function ClearPolygon() {
    	if (currentPolygon) {
  			currentPolygon.setMap(null);
		}
		ClearDataAlerts(); // clear any visible alerts about data of this polygon
		
		ClearChartData(); // delete any chart data (about the old region)
		
		jQuery('.region').removeClass('selected');
		
		// clear any polygon data displayed in any tab (data, chart etc)
		ClearPolygonData();

		// disable the selected region relative items
		EnableDisableSelectedRegionItems(false);

	}


	/**
	 * Clears the current polygon data from the related tabs (data, chart etc). 
	 */
	function ClearPolygonData() {

		// empty any previously selected region data text
		jQuery('.selected-region-data-details .result').empty();
		// empty any previously selected region download link
		jQuery('.selected-region-download-details .result').empty();

		// empty any previously selected region chart
		jQuery('#chart_div').empty();
		// empty any previously selected region chart data
		DataChart_XaxisValues = [];
		DataChart_YaxisValues = [];

	}
	

	/**
	 * Stores the current polygon drawn on the map and disables drawing.
	 * @param {Object} opt_overlay The new polygon drawn on the map. If
	 *     undefined, the default polygon is treated as the new polygon.
	 */
	function handleNewPolygon(opt_overlay) {
		currentPolygon = opt_overlay;
		jQuery('.region').addClass('selected');
		setDrawingModeEnabled(false);
		EnableDisableSelectedRegionItems(true);
	}


	/**
	 * Checks if the selected region polygon is drawn on the map. 
	 * @return {boolean} Whether selected region polygon is drawn.
	 */
	function isCurrentPolygonValid() {
		var ret = false
    	if (currentPolygon) {
  			if (currentPolygon.getMap() == map) {
				ret = true;
			}
		}
		return ret;
	}



	/**
	 * Focuses the map on the selected region polygon, if any.
	 */
	function FocusOnPolygon() {
		
		if (isCurrentPolygonValid() == true) {
			var latlngbounds = new google.maps.LatLngBounds();
	
			var vertices = currentPolygon.getPath();
			var xy;
			// Iterate over the vertices.
			for (var i =0; i < vertices.getLength(); i++) {
				xy = vertices.getAt(i);
				latlngbounds.extend(xy);
			}
			map.fitBounds(latlngbounds);
		}
	}
	


	
	
  	function _openSelectedFile() {               
 
 		var file, fr;
 
 			function receivedText() {
  				var triangleCoords = [];
  				var coords_text = '';
  				var coords = '';
  				var a = [];
  				var a_line = [];				
				var xml = fr.result;
				
				xmlDoc = jQuery.parseXML(xml);
  				xml = jQuery(xmlDoc);
  				coords = xml.find( "coordinates" );
				coords_text = coords.text();
				coords_text = coords_text.replace(/\n/g, ' '); // clear any newlines tabs
				coords_text = coords_text.replace(/\t/g, ' '); // clear any tabs
				coords_text = coords_text.replace(/'  '/g, ' '); // clear any double spaces
				a = coords_text.split(' ');
				for(var i=0;i<a.length;i++) {
					a_line = a[i].split(',');
					if (a_line.length == 3) {
						myLatLng = new google.maps.LatLng(a_line[1], a_line[0]);
						triangleCoords.push(myLatLng);
					}
				}


				if (isCurrentPolygonValid() == false) {
  					currentPolygon = new google.maps.Polygon({
		    			paths: triangleCoords,
    					strokeColor: '#FF0000',
    					strokeOpacity: 0.8,
		    			strokeWeight: 2,
    					fillColor: '#FF0000',
    					fillOpacity: 0.35
		  			});
					handleNewPolygon(currentPolygon);
				} else {
					currentPolygon.setPath(triangleCoords);
				}
				currentPolygon.setMap(map);
				FocusOnPolygon();
				
  			} // function receivedText()	  	
	
  
    	if (!window.File || !window.FileReader || !window.FileList || !window.Blob) {
      		alert('The File APIs are not fully supported in this browser.');
      		return;
    	}   

    	var input = document.getElementById('fileLoader');
    	if (!input) {
      		//alert("Um, couldn't find the fileinput element.");
    	} else if (!input.files) {
      		//alert("This browser doesn't seem to support the `files` property of file inputs.");
    	} else if (!input.files[0]) {
      		//alert("Please select a file before clicking 'Load'");               
    	} else {
      		file = input.files[0];
      		fr = new FileReader();
      		fr.onload = receivedText;
      		fr.readAsText(file);
     	// fr.readAsDataURL(file);
    	}


  	}




var _fileLoaderChangeFunctionIsDefined = false;

	
	function LoadPolygon() {
		
		if (_fileLoaderChangeFunctionIsDefined == false) {
       		jQuery("#fileLoader").change(function(e){
            	var fileName = e.target.files[0].name;
				//console.log('---- ' + e.target.files[0].name + ' -------');			
				_openSelectedFile();
        	});
		}
		_fileLoaderChangeFunctionIsDefined = true;
      	jQuery("#fileLoader").click();
	}




	function SavePolygon() {

		function cleanUp(a) {
  			a.textContent = 'Downloaded';
  			a.dataset.disabled = true;

	  	// Need a small delay for the revokeObjectURL to work properly.
  			setTimeout(function() {
    			window.URL.revokeObjectURL(a.href);
	  		}, 1500);
		}


		var container = document.querySelector('#select-region-panel');
		var output = container.querySelector('#polygon-save-output');
  		var prevLink = output.querySelector('a');
		
  			window.URL = window.webkitURL || window.URL;

  			if (prevLink) {
    			window.URL.revokeObjectURL(prevLink.href);
    			output.innerHTML = '';
  			}

			var kml_body = GetSelectedPolygonKmlDescription();
  			var bb = new Blob([kml_body], {type: KML_MIME_TYPE});

  			var a = document.createElement('a');
  			a.download = AppTitleOldFashion + '.kml';
  			a.href = window.URL.createObjectURL(bb);
//  			a.textContent = 'Download ready';
  			a.textContent = '';

  			a.dataset.downloadurl = [KML_MIME_TYPE, a.download, a.href].join(':');
  			a.draggable = true; // Don't really need, but good practice.
  			a.classList.add('dragout');


  			output.appendChild(a);

  			a.onclick = function(e) {
    			if ('disabled' in this.dataset) {
      				return false;
    			}
  			//  cleanUp(this);
  			};

		// Do simulate click
		a.click();

	}



	/**
	 * Returns the kml content that describes the selected polygon, if any.
	 */
	function GetSelectedPolygonKmlDescription() {
		
		var st = '';
		var coords = '';
		
		if (isCurrentPolygonValid() == true) {
//			var latlngbounds = new google.maps.LatLngBounds();
	
			var vertices = currentPolygon.getPath();
			var xy;
			if (vertices.getLength() > 0) {
				// Iterate over the vertices.
				for (var i =0; i < vertices.getLength(); i++) {
					xy = vertices.getAt(i);
					coords += String(xy.lng());
					coords += ',';
					coords += String(xy.lat());
					coords += ',0\n';
				}
				// append first item at the end to clode polygon
				xy = vertices.getAt(0);
				coords += String(xy.lng());
				coords += ',';
				coords += String(xy.lat());
				coords += ',0\n';

				st += '<?xml version="1.0" encoding="UTF-8"?> \n';
				st += '<kml xmlns="http://earth.google.com/kml/2.0"> <Document>\n';

				st += '<Placemark> \n';
				st += ' <Polygon> <outerBoundaryIs>  <LinearRing>  \n';
				st += '  <coordinates>\n';
				st += coords;
				st += '  </coordinates>\n';
				st += ' </LinearRing> </outerBoundaryIs> </Polygon>\n';
 
				st += ' <Style> \n';
				st += '  <PolyStyle>  \n';
				st += '   <color>#a00000ff</color>\n';
				st += '  <outline>0</outline>\n';
				st += '  </PolyStyle> \n';
				st += ' </Style>\n';
				st += '</Placemark>\n';

				st += '</Document>\n';
				st += '</kml>\n';

			} // if (vertices.getLength() > 0)


		}
		return st;
	}
	
	
	
	
	/**
	 * Returns a string with the coordinates of the selected region polygon.
	 * i.e. [27.0529,49.094],[26.9609,46.996],[30.219,47.7583],[28.609,48.977]
	 * @return {string} The string with the coordinates.
	 */
	function ExportPolygonCoords() {
		var ret = '';
		
		if (isCurrentPolygonValid() == true) {
			var vertices = currentPolygon.getPath();
			var xy;
			// Iterate over the vertices.
			for (var i =0; i < vertices.getLength(); i++) {
				xy = vertices.getAt(i);
				if (ret != '') { ret += ','; }
				ret += '[' + String(xy.lng()) + ',' + String(xy.lat()) + ']';
			}
		}
		return ret;
	}
	

	// ********************************* Selected Region Polygon *********************************
	// *******************************************************************************************



	function UpdateSelectedMapOverlay(){
    	var e = document.getElementById("BSAlbedoOptions_OverlayOption");
	    var f = e.options[e.selectedIndex].value;    
		var overlayName = e.options[e.selectedIndex].label;		
    	var aa = parseFloat(f);
		
			if (CurrentOverlayID != aa) {
				// a different coverage is selected
				// remove any remaining alert from screen
				removeAlert(AlertName_SelectedRegionChartDataRequest);
				removeAlert(AlertName_SelectedRegionDownloadRequest);
				removeAlert(AlertName_SelectedRegionDataRequest);
			}

			if (overlayName == '') {
				CurrentOverlayID = aa;
				CurrentOverlayName = overlayName;
				map.overlayMapTypes.setAt(0 , null);
				// Stop all region selection jobs
				ClearPolygon();
				CancelPolygon();
				ShowHideSelectRegionPanel(false);
				EnableDisableSelectedOverlayItems(false);
				CreateColorRamp(0, 0, ''); // NO color ramp
			} else {
				// Clear any displayed data about selected polygon in any tab (data, charts etc) 
				// for any previously selected coverage
				ClearPolygonData();
				GetMapID(aa, overlayName);
			
			}
	}



	function GetMapID(overlayID, overlayName) {

		var url = '/getmapid?layerid=' + String(overlayID);
		var MyTimeStamp = GetTimeStamp();
		LastTimeStampForMapID = MyTimeStamp;

		var AlertName = 'LoadingOverlay:' + overlayName;
  		setAlert(AlertName, 'warning', 'Overlay ' + overlayName, 'Receiving Overlay Information...');

		jQuery.ajax({
    		type: "GET",
    		url: url,
    		dataType: "json",
			timeout: MapIDInfoDataRequestTimeOut * 1000,
			error: function(){
  				setAlert(AlertName, 'danger', 'Overlay ' + overlayName, 'Receiving Overlay Information Failed.');
				removeAlert(AlertName);
				},
    		success: function(data){
				if (MyTimeStamp == LastTimeStampForMapID) {
					AddNewMapType(data.mapid, data.token);
					CurrentOverlayID = overlayID;
					CurrentOverlayName = overlayName;
  					setAlert(AlertName, 'success', 'Overlay ' + overlayName, 'Information Received Successfully.');
					removeAlert(AlertName);
					ShowHideSelectRegionPanel(true);
					EnableDisableSelectedOverlayItems(true);			
					//TODO: auto retrieve these values from the server
					CreateColorRamp(OverlayDataMinValue[overlayID], OverlayDataMaxValue[overlayID], OverlayDataPaletteValue[overlayID]);
				}
			} // success:
  		});
		
		
	}




	//dataType mean, max, min
	function requestDataForSelectedRegion(dataType, title, comments) {
		
		var msg;
		var coordinates = ExportPolygonCoords();
		if (coordinates != '') {
			var url = '/getdata?cmdid=' + dataType + '&layerid=' + CurrentOverlayID + '&coordinates=' + coordinates;
			var MyTimeStamp = GetTimeStamp();
			LastTimeStampForPoygonData = MyTimeStamp;
	
			var AlertName = AlertName_SelectedRegionDataRequest;
			if (comments != '') {
				msg = 'Data request (' + comments + ') for the selected region of overlay ' + CurrentOverlayName;
			} else {
				msg = 'Data request for the selected region of overlay ' + CurrentOverlayName;
			}
	  		setAlert(AlertName, 'warning', msg, 'waiting...');
			
			jQuery.ajax({
	    		type: "GET",
	    		url: url,
	    		dataType: "text",
				timeout: PolygonDataRequestTimeOut * 1000,
				error: function(){
	  				setAlert(AlertName, 'danger', msg, 'Failed.');
					removeAlert(AlertName);
					},
	    		success: function(data){
					if (MyTimeStamp == LastTimeStampForPoygonData) {
						if (isCurrentPolygonValid() == true) {
		  					setAlert(AlertName, 'success', 'Data for the selected region Received Successfully.');
							var data_str = JsonDataToHtml(data);
							if (title != '') {
								data_str = title + '<br />' + data_str;
							}
				      		jQuery('.selected-region-data-details .result').html(data_str);
							if (data_str != '') {
								// nothing
							}
						}
						removeAlert(AlertName);
					}
				} // success:
	  		});
		
		}
		
	}




	function requestDownloadForSelectedRegion() {
		
		var coordinates = ExportPolygonCoords();
		if (coordinates != '') {
			var url = '/download?layerid=' + CurrentOverlayID + '&coordinates=' + coordinates;
			var MyTimeStamp = GetTimeStamp();
			LastTimeStampForPoygonDownload = MyTimeStamp;
	
			var AlertName = AlertName_SelectedRegionDownloadRequest;
	  		setAlert(AlertName, 'warning', 'Request for URL to download the selected region of overlay ' + CurrentOverlayName, 'waiting...');
			
			jQuery.ajax({
	    		type: "GET",
	    		url: url,
	    		dataType: "text",
				timeout: PolygonDownloadRequestTimeOut * 1000,
				error: function(){
	  				setAlert(AlertName, 'danger', 'Request for URL to download the selected region of overlay ' + CurrentOverlayName, 'Failed.');
					removeAlert(AlertName);
					},
	    		success: function(data){
					if (MyTimeStamp == LastTimeStampForPoygonDownload) {
						if (isCurrentPolygonValid() == true) {
		  					setAlert(AlertName, 'success', 'Request for URL to download the selected region Received Successfully.');
							var data_str = JsonDataToHtml(data);
							var theHTML = JsonDataToHtml(data);
							theHTML = '<a href="' + theHTML + '" target="_blank">' + theHTML + '</a>';
				      		jQuery('.selected-region-download-details .result').html(theHTML);
							if (data_str != '') {
								// nothing
							}
						}
						removeAlert(AlertName);
					}
				} // success:
	  		});
		
		}
		
	}


	function getChartBoundsType() {
			
    	var e = document.getElementById("ChartBoundsOption");
	    var f = e.options[e.selectedIndex].value;    
		var overlayName = e.options[e.selectedIndex].label;		
    	var aa = parseFloat(f);
			
			return aa		
	}

	function getChartType() {
			
    	var e = document.getElementById("ChartTypeOption");
	    var f = e.options[e.selectedIndex].value;    
		var overlayName = e.options[e.selectedIndex].label;		
    	var aa = parseFloat(f);
			
			return aa		
	}


	function requestChartDataForSelectedRegion() {
		
		var coordinates = ExportPolygonCoords();
		if (coordinates != '') {
			
			var BoundsType = getChartBoundsType();
			var url;
			if (BoundsType == 0) { // Optimized
				url = '/chart?bounds=optimized&layerid=' + CurrentOverlayID + '&coordinates=' + coordinates;
			} else { // default
				url = '/chart?bounds=default&layerid=' + CurrentOverlayID + '&coordinates=' + coordinates;
			}
			
			var MyTimeStamp = GetTimeStamp();
			LastTimeStampForPoygonChartData = MyTimeStamp;
	
			var AlertName = AlertName_SelectedRegionChartDataRequest;
	  		setAlert(AlertName, 'warning', 'Request for Chart data about the selected region of overlay ' + CurrentOverlayName, 'waiting...');
			
			jQuery.ajax({
	    		type: "GET",
	    		url: url,
	    		dataType: "text",
				timeout: PolygonDownloadRequestTimeOut * 1000,
				error: function(){
	  				setAlert(AlertName, 'danger', 'Request for Chart data about the selected region of overlay ' + CurrentOverlayName, 'Failed.');
					removeAlert(AlertName);
					},
	    		success: function(data){
					if (MyTimeStamp == LastTimeStampForPoygonChartData) {
						if (isCurrentPolygonValid() == true) {
		  					setAlert(AlertName, 'success', 'Request for Chart data about the selected region Received Successfully.');
							var data_str = JsonDataToHtml(data);
							var row_data = postProcessJsonData(data);
							DataChart_YaxisValues = []; // initialize global data tables
							DataChart_XaxisValues = []; // initialize global data tables
							var aa = 0;
							var sArray = row_data.split(',');
							// fill DataChart_XaxisValues and DataChart_YaxisValues arrays with
							// numeric values 
							for(var k = 0 ; k < sArray.length; k++) {
								if (k > 0) { // info entries having the first item starting with info_ (ie info_count)
									a = sArray[k].split(':');
									DataChart_XaxisValues[aa] = a[0];
									DataChart_YaxisValues[aa] = parseFloat(a[1]);
									aa++;
								}
							}

							// format DataChart_XaxisValues array with numeric values intervals
							for(aa = DataChart_XaxisValues.length-1 ; aa >=0 ; aa--) {
								if ((aa > 0) && (aa < DataChart_XaxisValues.length-1)) {										
									DataChart_XaxisValues[aa] = DataChart_XaxisValues[aa-1] + ' to ' + DataChart_XaxisValues[aa];
								} else if (aa == 0) {
									DataChart_XaxisValues[aa] = 'less than ' + DataChart_XaxisValues[aa];
								} else if (aa == DataChart_XaxisValues.length-1) {
									DataChart_XaxisValues[aa] = 'greater than ' + DataChart_XaxisValues[aa-1];
								}
							}

							DataChart_Title = CurrentOverlayName;
							
				      		//jQuery('.selected-region-download-details .result').html(row_data);
				      		//jQuery('.selected-region-chart-details #chart_div').html(row_data);
							if (data_str != '') {
								CreateChart();
							}
						}
						removeAlert(AlertName);
					}
				} // success:
	  		});
		
		}
		
	}


	function ClearChartData() {
		
		DataChart_YaxisValues = [];
		DataChart_XaxisValues = [];
		DataChart_Title = '';
	}


	function EnableDisableSelectedRegionItems(buttons_enable) {	
			if (buttons_enable == true) {
				jQuery('#selected-region-data-panel .btn').removeAttr("disabled"); // all buttons of the data panel
				jQuery('#selected-region-chart-panel .btn').removeAttr("disabled"); // all buttons of the download panel
				jQuery(".selected-region-required-note").hide();
				jQuery("#tabs").tabs( "option", "disabled", [] );	
			} else {
				jQuery('#selected-region-data-panel .btn').attr("disabled", "disabled"); // all buttons of the data panel
				jQuery('#selected-region-chart-panel .btn').attr("disabled", "disabled"); // all buttons of the download panel
				jQuery(".selected-region-required-note").show();
				jQuery("#tabs").tabs( "option", "disabled", [ Data_Tab_ID, Charts_Tab_ID ] );				
			}
	}


	function EnableDisableSelectedOverlayItems(buttons_enable) {	
			if (buttons_enable == true) {
				jQuery(".selected-overlay-required-note").hide();
			} else {
				jQuery(".selected-overlay-required-note").show();
			}
	}


	function ShowHideSelectRegionPanel(display) {
			if (display == true) {
	    		jQuery('#select-region-panel').removeClass('hidden');
			} else {
	    		jQuery('#select-region-panel').addClass('hidden');
 			}
	}



	// Returns a timestamp in epoch format
	function GetTimeStamp() {
		var x = new Date().getTime();	
			
		return x;
	}


	
	function postProcessJsonData(data) {
		
		var strValue = '';
			strValue = data;
			if (strValue.substring(strValue.length - 1, strValue.length) == '}') {
				strValue = strValue.substring(0, strValue.length - 1);
			}
			strValue = replaceAll(strValue, ', u\'', ', \'');
			strValue = replaceAll(strValue, ',u\'', ',\'');
			strValue = replaceAll(strValue, '{u\'', '{\'');
		return strValue;
	}
	
	function JsonDataToHtml(data) {
		
		var strValue = '';
			strValue = data;
			if (strValue.substring(strValue.length - 1, strValue.length) == '}') {
				strValue = strValue.substring(0, strValue.length - 1);
			}
			strValue = replaceAll(strValue, ', u\'', '<br />\'');
			strValue = replaceAll(strValue, ',u\'', '<br />\'');
			strValue = replaceAll(strValue, '{u\'', '\'');
		return strValue;
	}
	
	
	
	
	function replaceAll(str, find, repl) {
  		return str.replace(new RegExp(escapeRegExp(find), 'g'), repl);
	}
	function escapeRegExp(str) {
    	return str.replace(/([.*+?^=!:${}()|\[\]\/\\])/g, "\\$1");
	}
	
	
	


	function Round(theNumber,theNumDigits) {

		var currentValue = theNumber;
		tmpPow = Math.pow(10, theNumDigits);
		currentValue = Math.round(currentValue * tmpPow) / tmpPow;
		return currentValue;
	}


	function CreateColorRamp(minValue, maxValue, paletteStr) {


		LastColorRampMinValue = minValue;
		LastColorRampMaxValue = maxValue;
		LastColorRampPaletteValue = paletteStr;

		var st = '';
		var paletteArray = paletteStr.split(',');			
		if ((minValue != maxValue) && (paletteStr != '') && (paletteArray.length > 1)) {
			var rampWidth = 500;
			rampWidth = jQuery('#ColorRamp').width();
			
			var mapWidth = jQuery('#map').width();
			
			if (mapWidth < rampWidth) {
				rampWidth = mapWidth;
			}
			
			var numberOfItems = 100;
			var theHint = 'Move the mouse pointer over the color ramp for colour values';
	
			var ItemWidthStr = String(parseInt(rampWidth/numberOfItems));
			var rainbow = new Rainbow(); 
			var minLabel = '', maxLabel = '';
			rainbow.setNumberRange(1, numberOfItems);
			rainbow.setSpectrum.apply(null, paletteArray); // rainbow.setSpectrum takes variable number of arguments

			var s = '';
			st = '<span style="width=100%"><table border="0" height="100%"><tr>';
			for (var i = 0; i <= numberOfItems; i++) {  // one more to get both boundary values
			    var hexColour = rainbow.colourAt(i);
			    s += '#' + hexColour + ', ';	
				currentValue = minValue + i * (maxValue - minValue) / numberOfItems;
				// round to six decimal digits
				currentValue = Round(currentValue,6); //Math.round(currentValue * 1000000) / 1000000;
				if (i == 0) { minLabel = String(currentValue); }
				if (i == numberOfItems) { maxLabel = String(currentValue); }
				actions = 'onMouseOver=\'tooltip.show("data value: ' + String(currentValue) +'");\' onMouseOut=\'tooltip.hide();\'';
				st += '<td ' + actions + ' class="ColorRampCell" width="' + ItemWidthStr + '" bgcolor="#' + hexColour + '"></td>';
			}
		
			st += '</tr>';
		
			st += '<tr>';
			st += '<td colspan="' + String(numberOfItems + 1) + '">';
			
			if (rampWidth < 300) { // too short
				theHint = '';
			}
			
			st += '<table border="0" width="100%"><tr><td>' + minLabel + '</td><td class="centered_table_cell" width="100%">' + theHint + '</td><td>' + maxLabel + '</td></tr></table>';
			st += '</td>';
			st += '</tr>';
		
			st += '</table></span>';
		}
		
    	var e = document.getElementById("ColorRamp");
		e.innerHTML = st;

		ResizeMapDivs(jQuery(window).width(), jQuery(window).height());

	}



	function MoveDividerLeft() {
		var map = document.getElementById('map');
		var exporter = document.getElementById('exporter');
		var map_width = jQuery('#map').width();
		if (map_width > LeftWrapperMinWidth) {
			map_width = map_width - 100;
			if (map != null) {
				map.style.width = String(map_width)+'px';
			}
			if (exporter != null) {
				exporter.style.width = String(map_width)+'px';
			}
		
			ResizeMapDivs(jQuery(window).width(), jQuery(window).height());
			if (LastColorRampMinValue != LastColorRampMaxValue) {
				// there IS a colour ramp
				CreateColorRamp(LastColorRampMinValue, LastColorRampMaxValue, LastColorRampPaletteValue);
			}
			RefreshMap();
		}
	}

	function MoveDividerRight() {
		var map = document.getElementById('map');
		var exporter = document.getElementById('exporter');
		var map_width = jQuery('#map').width();
		if (jQuery(window).width() - map_width > RightWrapperMinWidth) {
			map_width = map_width + 100;
			if (map != null) {
				map.style.width = String(map_width)+'px';
			}
			if (exporter != null) {
				exporter.style.width = String(map_width)+'px';
			}
		
			ResizeMapDivs(jQuery(window).width(), jQuery(window).height());
			if (LastColorRampMinValue != LastColorRampMaxValue) {
				// there IS a colour ramp
				CreateColorRamp(LastColorRampMinValue, LastColorRampMaxValue, LastColorRampPaletteValue);
			}
			RefreshMap();
		}
	}

	// Refresh the map after markers adding, etc
	function RefreshMap() {
		
		google.maps.event.trigger(map, 'resize');
		map.setZoom( map.getZoom()-1 );		
		map.setZoom( map.getZoom()+1 );		
	}
	

	// Resize Webpage Divs
	function ResizeMapDivs(newWidth, newHeight) {
		var wrapper = document.getElementById('wrapper');
		var wrapper_left = document.getElementById('wrapper-left');
		var wrapper_right = document.getElementById('wrapper-right');
		var wrapper_top = document.getElementById('wrapper-top');
		var exporter = document.getElementById('exporter');
		var tabs = document.getElementById('tabs');
		var map = document.getElementById('map');
		var divider = document.getElementById('divider');
		var ColorRamp = document.getElementById('ColorRamp');
		var chart_div = document.getElementById('chart_div');
		
		
		var wrapper_bottom = document.getElementById('wrapper-bottom');

		//newHeight = newHeight - 10;
		
		var ExtraHeight = 10;
		//if (wrapper != null) {wrapper.style.height = String(newHeight)+'px';}

		var wrapper_Height = jQuery('#wrapper').height();
		

		if (wrapper != null) {wrapper.style.height = String(newHeight)+'px';}
		wrapper_Height = newHeight; 
		
		var wrapper_left_height = newHeight - jQuery('#wrapper-top').height() - jQuery('#wrapper-bottom').height();
		var wrapper_left_width = jQuery('#exporter').width();
		if (wrapper_left != null) {
			wrapper_left.style.width = String(wrapper_left_width)+'px';
			wrapper_left.style.height = String(wrapper_left_height)+'px';
		}

		var exporter_height = wrapper_left_height - jQuery('#wrapper-left-title-div').height() - 10;
		if (exporter != null) {
			exporter.style.height = String(exporter_height)+'px';
		}

		var ColorRamp_height = jQuery('#ColorRamp').height();
		if (ColorRamp_height > 0) { ColorRamp_height += 5; }
		var map_height = exporter_height - ColorRamp_height;
		if (map != null) {
			map.style.width = String(jQuery('#exporter').width())+'px';
			map.style.height = String(map_height)+'px';
		}

		var ColorRamp_style_width = jQuery('#exporter').width();
		if (ColorRamp_style_width > ColorRampMaxWidth) {
			ColorRamp_style_width = ColorRampMaxWidth;
		}
		if (ColorRamp != null) {
			ColorRamp.style.width = String(ColorRamp_style_width)+'px';
		}


		var divider_width = jQuery('#divider').width();
		var divider_height = newHeight - jQuery('#wrapper-top').height() - jQuery('#wrapper-bottom').height();
		if (divider != null) {
			divider.style.height = String(divider_height)+'px';
		}

		if (wrapper != null) {
			wrapper.style.minWidth = String(wrapper_left_width + divider_width + RightWrapperMinWidth)+'px';
		}

		var wrapper_right_width = newWidth - jQuery('#wrapper-left').width() - divider_width - 20;
		var wrapper_right_height = newHeight - jQuery('#wrapper-top').height() - jQuery('#wrapper-bottom').height();
		if (wrapper_right != null) {
			if (wrapper_right_width < RightWrapperMinWidth) {
				wrapper_right_width = RightWrapperMinWidth;
			}
			wrapper_right.style.width = String(wrapper_right_width)+'px';
			wrapper_right.style.height = String(wrapper_right_height)+'px';
		}
		
		var tabs_height = wrapper_right_height - jQuery('#wrapper-right-title-div').height();
		if (tabs != null) {
			tabs.style.width = String(wrapper_right_width)+'px';
			tabs.style.height = String(tabs_height)+'px';
		}

		var theight = tabs_height - jQuery('#tabs_ul').height() - 15;	
		jQuery('.AppTabPanel').css("height", String(theight) + 'px'); 
		var twidth = wrapper_right_width - 10;
		jQuery('.AppTabPanel').css("width", String(twidth) + 'px'); 

		if (chart_div != null) {
			var prev_chart_div_height = jQuery('#chart_div').height();
			var prev_chart_div_width = jQuery('#chart_div').width();
			chart_div.style.height = String(theight - jQuery('#selected-region-chart-title').actual('height') - jQuery('#selected-region-chart-toolbar').actual('height') - 15) + 'px';
			chart_div.style.width = String(twidth) + 'px';

			if ((prev_chart_div_height != jQuery('#chart_div').actual('height')) || (prev_chart_div_width != jQuery('#chart_div').actual('width'))) {
				CreateChart();
			}
		}


	}


	function AppTabClicked(tabID) {
		if (tabID == Charts_Tab_ID) {
		}
	}

 
	function CreateChart() {

   		if ((DataChart_XaxisValues.length > 0) && (DataChart_YaxisValues.length > 0)) {
 
	  		var data = new google.visualization.DataTable();
			data.addColumn('string', 'value');
			data.addColumn('number', 'pixels');

			for(i = 0; i < DataChart_XaxisValues.length; i++)
				data.addRow([DataChart_XaxisValues[i], DataChart_YaxisValues[i]]);
				

			var chart_height = jQuery('#chart_div').actual('height') - 50; // for chart title, etc
			var chart_width = jQuery('#chart_div').actual('width');

			var options = {
    			title: DataChart_Title,
    			legend: { position: 'none' },
    			colors: ['#4285F4'],
				//curveType: 'function', //smooth lines
    			chartArea: { 
					width: chart_width, 
					height: chart_height, 
					},
				pointSize: 4,

				ticks: [0, 1],
				
				vAxis: {
                    baselineColor: '#f00',
                    gridlines: {color: '#0f0'},                        
                },
    			hAxis: {
      				textPosition: 'none'
    			},
    			bar: { gap: 0 },
  			}	


			var ChartType = getChartType();

			// Create and draw the visualization.
			if (ChartType == 0) {
				new google.visualization.ColumnChart(document.getElementById('chart_div')).draw(data, options);
			} else {
				new google.visualization.LineChart(document.getElementById('chart_div')).draw(data, options);
			}
			//new google.visualization.BarChart(document.getElementById('chart_div')).draw(data, options);
		}
	}