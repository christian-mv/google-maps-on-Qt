# Google maps plugin on Qt location.
This repo shows how to add a customed google-maps plugin to your Qt app without using any web viewer but using the Qt location API. 

![demostration gif](https://github.com/christian-mv/googlemaps-on-qml/blob/main/google-maps-qml-gif.gif?raw=true)

For this tutorial you need both a [google API KEY](https://developers.google.com/maps/documentation/javascript/get-api-key) 
and a third party [repository](https://github.com/vladest/googlemaps):

1 - Get your Goolge API KEY [here](https://developers.google.com/maps/documentation/javascript/get-api-key)

2 - Clone the googlemaps third-party repository from [here](https://github.com/vladest/googlemaps)

Please compile the source code in both debug and release mode. 
Then copy the compiled binaries into the appropriate Qt plugin folders which in windows might be something like this: 
C:\Qt\5.15.2\msvc2019_64\plugins\geoservices

Create a Qt/Qml project and add the following code. Notice where your API-KEY goes:

```
import QtQuick 2.15
import QtQuick.Window 2.15
import QtLocation 5.12
import QtPositioning 5.12
import QtQuick.Controls 2.15

Window {
    width: 640
    height: 480
    visible: true

    Plugin {
        id: myPlugin
        name:"googlemaps"
        PluginParameter {
            name: "api"
            value: "YOUR API KEY GOES HERE"
        }
    }
    Map {
        anchors.fill: parent
        id: map
        plugin:myPlugin
        activeMapType: map.supportedMapTypes[1]

        Component.onCompleted:{
            let mapLayers = [];
            for(var i=0; i<map.supportedMapTypes.length; i++)
                mapLayers.push( {"name": map.supportedMapTypes[i].name, "value":map.supportedMapTypes[i]} );

            _comboBox.model = mapLayers;
        }
    }
    ComboBox {
        id: _comboBox
        textRole: "name"
        valueRole: "value"
        onCurrentValueChanged: map.activeMapType = currentValue;

    }
}
```
