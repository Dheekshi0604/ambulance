# ambulance
I t is an application used in an emergency situation. Where users can contact the hospital quickly and can help the person immediately who is in need for first aid medicine and treatment. It can be accessed with our mobile since its an application it can perform several tasks. 
code:
User App Ambulance:
Home page:
packagecom.user.ambulance;
importjava.util.ArrayList;
importandroid.os.Bundle;
importandroid.app.Activity;
importandroid.app.Fragment;
importandroid.app.FragmentManager;
importandroid.content.Intent;
importandroid.content.res.Configuration;
importandroid.content.res.TypedArray;
import android.support.v4.app.ActionBarDrawerToggle;
import android.support.v4.widget.DrawerLayout;
importandroid.util.Log;
importandroid.view.Menu;
importandroid.view.MenuItem;
importandroid.view.View;
importandroid.widget.AdapterView;
importandroid.widget.ListView;
publicclassMainActivityextends Activity {
	privateDrawerLayoutmDrawerLayout;
	privateListViewmDrawerList;
	privateActionBarDrawerTogglemDrawerToggle;
	privateCharSequencemDrawerTitle;
privateCharSequencemTitle;
private String[] navMenuTitles;
	privateTypedArraynavMenuIcons;
privateArrayList<NavDrawerItem>navDrawerItems;
	privateNavDrawerListAdapteradapter;
	@Override
	protectedvoidonCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		mTitle = mDrawerTitle = getTitle();
		// getting items of slider from array
navMenuTitles = getResources().getStringArray(R.array.nav_drawer_items);
// getting Navigation drawer icons from res
navMenuIcons = getResources().obtainTypedArray(R.array.nav_drawer_icons);
mDrawerLayout = (DrawerLayout) findViewById(R.id.drawer_layout);
	mDrawerList = (ListView) findViewById(R.id.list_slidermenu);
navDrawerItems = newArrayList<NavDrawerItem>();

		// list item in slider at 1 Home Nasdaq details
navDrawerItems.add(newNavDrawerItem(navMenuTitles[0], navMenuIcons
				.getResourceId(0, -1)));
		// list item in slider at 2 Facebook details
navDrawerItems.add(newNavDrawerItem(navMenuTitles[1], navMenuIcons
				.getResourceId(1, -1)));
		// list item in slider at 3 Google details
navDrawerItems.add(newNavDrawerItem(navMenuTitles[2], navMenuIcons
				.getResourceId(2, -1)));
		// list item in slider at 4 Apple details
navDrawerItems.add(newNavDrawerItem(navMenuTitles[3], navMenuIcons
				.getResourceId(3, -1)));
		// list item in slider at 5 Microsoft details
navDrawerItems.add(newNavDrawerItem(navMenuTitles[4], navMenuIcons
				.getResourceId(4, -1)));
navDrawerItems.add(newNavDrawerItem(navMenuTitles[5], navMenuIcons
				.getResourceId(5, -1)));
navDrawerItems.add(newNavDrawerItem(navMenuTitles[6], navMenuIcons
				.getResourceId(6, -1)));
		// list item in slider at 6 LinkedIn details
		// navDrawerItems.add(new NavDrawerItem(navMenuTitles[5],
		// navMenuIcons.getResourceId(5, -1)));

		// Recycle array
		navMenuIcons.recycle();
mDrawerList.setOnItemClickListener(newSlideMenuClickListener());

		// setting list adapter for Navigation Drawer
		adapter = newNavDrawerListAdapter(getApplicationContext(),
				navDrawerItems);
		mDrawerList.setAdapter(adapter);

		// Enable action bar icon_luncher as toggle Home Button
		getActionBar().setDisplayHomeAsUpEnabled(true);
		getActionBar().setHomeButtonEnabled(true);

		mDrawerToggle = newActionBarDrawerToggle(this, mDrawerLayout,
			R.drawable.ic_drawer, R.string.app_name, R.string.app_name) {

			publicvoidonDrawerClosed(View view) {
				getActionBar().setTitle(mTitle);
			invalidateOptionsMenu(); // Setting, Refresh and Rate App
			}

			publicvoidonDrawerOpened(View drawerView) {
				getActionBar().setTitle(mDrawerTitle);
				invalidateOptionsMenu();
			}
		};
		mDrawerLayout.setDrawerListener(mDrawerToggle);

		if (savedInstanceState == null) {
			displayView(0);
		}
	}



Driver App Ambulance:
//Home Page:
packagecom.user.ambulance;
importjava.io.BufferedReader;
importjava.io.IOException;
importjava.io.InputStream;
importjava.io.InputStreamReader;
importjava.net.HttpURLConnection;
import java.net.URL;
importjava.util.HashMap;
importjava.util.List;
importorg.json.JSONObject;
importcom.google.android.gms.maps.CameraUpdateFactory;
importcom.google.android.gms.maps.GoogleMap;
importcom.google.android.gms.maps.MapFragment;
import com.google.android.gms.maps.GoogleMap.OnInfoWindowClickListener;
importcom.google.android.gms.maps.model.BitmapDescriptorFactory;
importcom.google.android.gms.maps.model.CameraPosition;
importcom.google.android.gms.maps.model.LatLng;
importcom.google.android.gms.maps.model.Marker;
importcom.google.android.gms.maps.model.MarkerOptions;
importandroid.app.AlertDialog;
importandroid.app.Fragment;
importandroid.content.Context;
importandroid.content.DialogInterface;
importandroid.content.Intent;
importandroid.content.SharedPreferences;
importandroid.content.SharedPreferences.Editor;
importandroid.location.Criteria;
importandroid.location.Location;
importandroid.location.LocationListener;
importandroid.location.LocationManager;
importandroid.os.AsyncTask;
importandroid.os.Bundle;
importandroid.util.Log;
importandroid.view.LayoutInflater;
importandroid.view.Menu;
importandroid.view.MenuItem;
importandroid.view.View;
importandroid.view.ViewGroup;
importandroid.view.View.OnClickListener;
importandroid.widget.Button;
importandroid.widget.EditText;
importandroid.widget.Toast;

publicclassNearestDoctorextends Fragment implementsLocationListener {
	privatestaticfinal String LOCATION_SERVICE = null;
	// Google Map
	privateGoogleMapgoogleMap;
	GPSTrackergps;
	doublelatitude, longitude;
doublemLatitude = 0;
	doublemLongitude = 0;
	SharedPreferencespref;
HashMap<String, String>mMarkerPlaceLink = newHashMap<String, String>();
Button btnFind;
publicNearestDoctor() {
}
	public View onCreateView(LayoutInflaterinflater, ViewGroup container,
			Bundle savedInstanceState) {
View rootView = inflater.inflate(R.layout.nearest_doctor, container,false);
try {
			initilizeMap();
			googleMap.setMapType(GoogleMap.MAP_TYPE_NORMAL);
			googleMap.setMyLocationEnabled(true);
googleMap.getUiSettings().setZoomControlsEnabled(false);
			googleMap.getUiSettings().setMyLocationButtonEnabled(true);
			googleMap.getUiSettings().setCompassEnabled(true);
			googleMap.getUiSettings().setRotateGesturesEnabled(true);
			googleMap.getUiSettings().setZoomGesturesEnabled(true);
			gps = newGPSTracker(getActivity());
			if (gps.canGetLocation()) {
				Log.d("Your Location", "latitude:" + gps.getLatitude()
						+ ", longitude: " + gps.getLongitude());
				latitude = gps.getLatitude();
				longitude = gps.getLongitude();
}
MarkerOptions marker = newMarkerOptions().position(
			newLatLng(latitude, longitude)).title("Current Location ");
marker.icon(BitmapDescriptorFactory
			.defaultMarker(BitmapDescriptorFactory.HUE_BLUE));
googleMap.addMarker(marker);
CameraPositioncameraPosition = newCameraPosition.Builder()
			.target(newLatLng(latitude, longitude)).zoom(15).build();
googleMap.animateCamera(CameraUpdateFactory
					.newCameraPosition(cameraPosition));
			btnFind = (Button) rootView.findViewById(R.id.btn_find);
		LocationManagerlocationManager = (LocationManager) getActivity()
				.getSystemService(Context.LOCATION_SERVICE);
			Criteria criteria = newCriteria();
String provider = locationManager.getBestProvider(criteria, true);

	Location location = locationManager.getLastKnownLocation(provider);
if (location != null) {
				onLocationChanged(location);
			}
locationManager.requestLocationUpdates(provider, 20000, 0, this);
googleMap.setOnInfoWindowClickListener(newOnInfoWindowClickListener() 
{
			publicvoidonInfoWindowClick(Marker arg0) {
			Intent intent = newIntent(getActivity(),PlacesDetails.class);
			String reference = mMarkerPlaceLink.get(arg0.getId());
			intent.putExtra("reference", reference);
			System.out.println(" sender Refrencebunndle "+ reference);
startActivity(intent);
}
			});
			btnFind.setOnClickListener(newOnClickListener() {
				publicvoidonClick(View v) {
					String rad = "3000";
					Toast.makeText(getActivity(),
	String.valueOf("Latitude :" + mLatitude + " "+ "Longitude :"mLongitude),
							Toast.LENGTH_LONG).show();
		StringBuildersb = newStringBuilder(
		"https://maps.googleapis.com/maps/api/place/nearbysearch/json?");
		sb.append("location=" + mLatitude + "," + mLongitude);
					sb.append("&radius=" + rad);
					sb.append("&types=doctor");
					sb.append("&sensor=true");		sb.append("&key=AIzaSyCeMwhKwQfD6Zc1cS8iuQ_sK9QwnCjwYak");
String s = sb.toString();
					Log.e("Results", s);
					System.out.println("Results" + "  " + s + "  " + sb);
					PlacesTaskplacesTask = newPlacesTask();
				placesTask.execute(sb.toString());
}
			});

		} catch (Exception e) {
			e.printStackTrace();
		}
		returnrootView;
}

