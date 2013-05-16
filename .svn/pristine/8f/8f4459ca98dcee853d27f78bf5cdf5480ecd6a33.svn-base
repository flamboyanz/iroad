
 var s_map,
    s_currentPosition,
    s_directionsDisplay, 
    s_directionsService, s_places, s_iw;
    var s_markers = [];
    var s_autocomplete;

$(document).on("pageshow", "#service-locator", function() {
   navigator.geolocation.getCurrentPosition(serviceSuccess, ServiceError);
   $(".s_direction_close a").click(function(){
        $("#s_directions").hide();
    });

  

  
});

$(document).on("pagehide", "#service-locator", function() {    
  // resetVariables();
});

function ServiceError(error) {
     ServiceInitialize(59.3426606750, 18.0736160278);
  }

function serviceSuccess(position) {
    ServiceInitialize(position.coords.latitude, position.coords.longitude);
}

function ServiceInitialize(lat, lon){
    $('#service_map_canvas').css({height:(screen.height-200)}); 

    s_directionsDisplay = new google.maps.DirectionsRenderer(); 
    s_directionsService = new google.maps.DirectionsService();
    s_currentPosition = new google.maps.LatLng(lat, lon);

    s_map = new google.maps.Map(document.getElementById('service_map_canvas'), {
       zoom: 14,
       center: s_currentPosition,
       mapTypeId: google.maps.MapTypeId.ROADMAP
     });

     var currentPositionMarker = new google.maps.Marker({
        position: s_currentPosition,
        map: s_map,
        title: "Current position"
    });

    s_places = new google.maps.places.PlacesService(s_map);
    s_directionsDisplay.setMap(map);

   google.maps.event.addListener(s_map, 'tilesloaded', function(){
      google.maps.event.clearListeners(s_map, 'tilesloaded');
      google.maps.event.addListener(s_map, 'zoom_changed', service_search);
      google.maps.event.addListener(s_map, 'dragend', service_search);
      service_search();
  });

  $("#menu :checkbox").click(function(){
    if (this.checked){
      service_search();
    } else {
      s_clearMarkers();
    }
  }); 

  /* $("#menu .custom").click(function(){
      service_search();
  })*/

  function service_search(){
    var search_type = ['car_dealer', 'car_repair','car_rental'];
    var randomItem = search_type[Math.floor(Math.random()*search_type.length)]
    var type = randomItem;//$("#service_checkbox").val();
      var search = {
          bounds: s_map.getBounds()
        };

        if (type != 'establishment') {
          search.types = [type];
        }

        s_places.search(search, function(s_results, s_status) {
          if (s_status == google.maps.places.PlacesServiceStatus.OK) {
            s_clearMarkers();
            
            for (var i = 0; i < s_results.length; i++) {
             // alert(s_results[i].geometry.location);
              var s_image = 'images/pin.png'; 
              s_markers[i] = new google.maps.Marker({
                position: s_results[i].geometry.location,
                icon: s_image


               
              });
              google.maps.event.addListener(s_markers[i], 'click', s_getDetails(s_results[i], i));
              setTimeout(s_dropMarker(i), i * 100);             
            }
          }
        });


  }

}

 function s_tilesLoaded() {
        google.maps.event.clearListeners(s_map, 'tilesloaded');
        google.maps.event.addListener(s_map, 'zoom_changed', service_search);
        google.maps.event.addListener(s_map, 'dragend', service_search);
        service_search();
    }

function s_dropMarker(i) {
return function() {
  s_markers[i].setMap(s_map);
}
}

function s_getDetails(s_result, i) {
    var s_end = new google.maps.LatLng(s_result.geometry.location.Ya, s_result.geometry.location.Za);
    
    return function() {
      s_places.getDetails({
        reference: s_result.reference
      }, s_showInfoWindow(i, s_end));             
    }
}

function s_showInfoWindow(i, s_end) {
  s_calculateRoute(s_end);
  $directions_width =  $("#s_directions").width();
  $dir_width = ($(document).width()- $directions_width)/2;
  $(".s_direction_close").css({left:$("#s_directions").width()-20+'px', position: 'relative'});
  $("#s_directions").css({left: $dir_width+'px', top: '150px'}).show();
 
}

function s_calculateRoute(destiny) {                
        var targetDestination = destiny;                    
        if (s_currentPosition && s_currentPosition != '' && targetDestination && targetDestination != '') {
            var request = {
                origin:s_currentPosition, 
                destination:targetDestination,
                travelMode: google.maps.DirectionsTravelMode["DRIVING"]
            };

            s_directionsService.route(request, function(s_response, s_status) {
                if (s_status == google.maps.DirectionsStatus.OK) {
                    s_directionsDisplay.setPanel(document.getElementById("s_directions_inner"));
                    s_directionsDisplay.setOptions( {
                         suppressMarkers: true ,
                         preserveViewport: true,
                         draggable: true,
                         suppressInfoWindows: false
                     } );
                    s_directionsDisplay.setDirections(s_response); 
                    
                  
                    $("#results").show();
                    
                }
                else {
                    $("#results").hide();
                }
            });
        }
        else {
            $("#results").hide();
        }
        
    }
function s_clearMarkers() {
for (var i = 0; i < s_markers.length; i++) {
  if (s_markers[i]) {
    s_markers[i].setMap(null);
    s_markers[i] == null;
  }
}
}
