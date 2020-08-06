/* global google */
import {
  default as React,
  Component,
} from "react";


import withGoogleMap from './assets/withGoogleMap';
import GoogleMap from './assets/GoogleMap';
import Marker from './assets/Marker';

const SimpleClickEventExampleGoogleMap = withGoogleMap(props => (
  <GoogleMap
    ref={props.onMapMounted}
    zoom={props.zoom}
    center={props.center}
    onCenterChanged={props.onCenterChanged}
  >
    <Marker
      defaultPosition={props.center}
      title="Click to zoom"
      onClick={props.onMarkerClick}
    />

    <Marker
      defaultPosition={props.marker2center}
      title="Click to zoom"
      onClick={props.onMarkerClick}
    />



  </GoogleMap>
));

const INITIAL_CENTER = { lat: 48.8566, lng: 2.3522 };
const MARKER1_CENTER = { lat: 48.8566, lng: 2.3522 };
const MARKER2_CENTER = { lat: 30.3752, lng: 76.7821 };

/*
 * https://developers.google.com/maps/documentation/javascript/examples/event-simple
 *
 * Add <script src="https://maps.googleapis.com/maps/api/js"></script> to your HTML to provide google.maps reference
 */
export default class SimpleClickEventExample extends Component {



  state = {
    zoom: 4,
    center: INITIAL_CENTER,
    marker2center: MARKER2_CENTER
  };

  handleMapMounted = this.handleMapMounted.bind(this);
  handleCenterChanged = this.handleCenterChanged.bind(this);
  handleMarkerClick = this.handleMarkerClick.bind(this);

  handleMapMounted(map) {
    this._map = map;
  }

  handleMarkerClick() {
    this.setState({
      zoom: 8,
    });
  }

  handleCenterChanged() {
    const nextCenter = this._map.getCenter();
    if (nextCenter.equals(new google.maps.LatLng(INITIAL_CENTER))) {
      return;
    }
    if (this._timeoutId) {
      clearTimeout(this._timeoutId);
    }
    this._timeoutId = setTimeout(() => {
      this.setState({ center: INITIAL_CENTER });
      this._timeoutId = null;
    }, 3000);

    this.setState({
      center: nextCenter,
    });
  }

  componentWillUnmount() {
    if (this._timeoutId) {
      clearTimeout(this._timeoutId);
    }
  }

  render() {
    return (
      <div className='googleMap' style={{width: "500px", height: "500px", margin: "0 auto"}}>
        <SimpleClickEventExampleGoogleMap
          containerElement={
            <div style={{ height: `100%` }} />
          }
          mapElement={
            <div style={{ height: `100%` }} />
          }
          zoom={this.state.zoom}
          center={this.state.center}
          onMapMounted={this.handleMapMounted}
          onCenterChanged={this.handleCenterChanged}
          onMarkerClick={this.handleMarkerClick}
        />
      </div>
    );
  }
}
