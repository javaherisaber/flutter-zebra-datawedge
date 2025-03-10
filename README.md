# zebra_datawedge
NOTICE: This package is forked from [here](https://github.com/mwaylabs/flutter-zebra-datawedge) to support for null-safety

`Only for Android.`
Based on this example: https://github.com/darryncampbell/DataWedge-Intent-Example-1

This package was created to support hardware scanner for Zebra devices (ET5X) with older DataWedge Version (^5.0).
Providing a simple stream which contains events fired by DataWedge (5.0).

For sure this is a minimal solution as it focus only to support DataWedge version 5.0.
With never DataWedge versions there are more possibilities and also prettier solutions.
Like creating incode the necessary profile.

# Profile Configuration

As described in the previous written link, you have to set up your profile configuration properly to be able listen for the DataWedge Intent.

1. go to the DataWedge application and create a new `profile`for our application or modify the `Profile0`.
2. Enable the profile
3. Enable `Barcode input`
4. Enable `Intent output`
5. Configure the `Intent output`as follows:
   1. `Intent action` = `io.mway.flutter_zebra_datawedge.ACTION`
   2. leave `Intent category` empty
   3. `Intent delivery` = `Broadcast`

# Example

```dart
  String _data = "waiting...";
  String _labelType = "waiting...";
  String _source = "waiting...";

  // create a listener for data wedge package
  Future<void> initDataWedgeListener() async {
    DataWedgeIntent.listenForDataWedgeEvent((response) {
      if (response != null && response is String)
        setState(() {
          Map<String, dynamic> jsonResponse;
          try {
            jsonResponse = json.decode(response);
          } catch (e) {
            //TODO handling
          }
          if (jsonResponse != null) {
            _data = jsonResponse["decodedData"];
            _labelType = jsonResponse["decodedLabelType"];
            _source = jsonResponse["decodedSource"];
          } else {
            _source = "An error occured";
          }
        });
    });
  }
```
