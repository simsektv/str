package com.phantom.simsek;

import android.annotation.SuppressLint;
import android.content.pm.ActivityInfo;
import android.content.pm.PackageInfo;
import android.content.pm.PackageManager;
import android.content.pm.Signature;
import android.content.res.Configuration;
import android.graphics.Bitmap;
import android.graphics.Color;
import android.graphics.PorterDuff;
import android.os.AsyncTask;
import android.util.Log;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.content.ContextCompat;
import androidx.fragment.app.Fragment;
import androidx.fragment.app.FragmentManager;
import androidx.fragment.app.FragmentPagerAdapter;
import androidx.viewpager.widget.ViewPager;

import okhttp3.FormBody;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.RequestBody;
import okhttp3.Response;

import android.content.Context;
import android.content.Intent;
import android.content.SharedPreferences;
import android.net.Uri;
import android.os.Build;
import android.os.Bundle;
import android.os.Handler;
import android.view.Gravity;
import android.view.KeyEvent;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.view.WindowManager;
import android.widget.ImageButton;
import android.widget.LinearLayout;
import android.widget.ProgressBar;
import android.widget.TextView;
import android.widget.Toast;

import com.google.android.exoplayer2.C;
import com.google.android.exoplayer2.ExoPlaybackException;
import com.google.android.exoplayer2.ExoPlayer;
import com.google.android.exoplayer2.ExoPlayerFactory;
import com.google.android.exoplayer2.SimpleExoPlayer;
import com.google.android.exoplayer2.Timeline;
import com.google.android.exoplayer2.ext.okhttp.OkHttpDataSourceFactory;
import com.google.android.exoplayer2.extractor.ts.DefaultTsPayloadReaderFactory;
import com.google.android.exoplayer2.source.MediaSource;
import com.google.android.exoplayer2.source.MergingMediaSource;
import com.google.android.exoplayer2.source.ProgressiveMediaSource;
import com.google.android.exoplayer2.source.SingleSampleMediaSource;
import com.google.android.exoplayer2.source.dash.DashMediaSource;
import com.google.android.exoplayer2.source.dash.DefaultDashChunkSource;
import com.google.android.exoplayer2.source.hls.DefaultHlsExtractorFactory;
import com.google.android.exoplayer2.source.hls.HlsMediaSource;
import com.google.android.exoplayer2.source.smoothstreaming.DefaultSsChunkSource;
import com.google.android.exoplayer2.source.smoothstreaming.SsMediaSource;
import com.google.android.exoplayer2.trackselection.AdaptiveTrackSelection;
import com.google.android.exoplayer2.trackselection.DefaultTrackSelector;
import com.google.android.exoplayer2.ui.AspectRatioFrameLayout;
import com.google.android.exoplayer2.ui.PlayerControlView;
import com.google.android.exoplayer2.ui.SimpleExoPlayerView;
import com.google.android.exoplayer2.upstream.DataSource;
import com.google.android.exoplayer2.upstream.DefaultBandwidthMeter;
import com.google.android.exoplayer2.upstream.DefaultDataSourceFactory;
import com.google.android.exoplayer2.util.MimeTypes;
import com.google.android.exoplayer2.util.Util;
import com.google.android.material.tabs.TabLayout;
import com.phantom.simsek.horizontal.HorizontalView;

import org.jetbrains.annotations.NotNull;
import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import java.io.IOException;
import java.net.NetworkInterface;
import java.net.SocketException;
import java.security.MessageDigest;
import java.util.Collections;
import java.util.Objects;
import java.util.Timer;
import java.util.TimerTask;

import static maes.tech.intentanim.CustomIntent.customType;

public class VideoPlayerActivity extends AppCompatActivity implements ExoPlayer.EventListener, PlayerControlView.VisibilityListener {
    private SimpleExoPlayerView exoPlayerView;
    private SimpleExoPlayer exoPlayer = null;
    private DefaultBandwidthMeter bandwidthMeter;
    private DefaultTrackSelector trackSelector;
    private MediaSource mediaSources;
    private String He1Key, He2Key, He3Key, He4Key;
    /*-------------------------------*/
    private String He1Val, He2Val, He3Val, He4Val;
    private String userAgent;
    private TextView exoTitle, player_hata_text;
    private ProgressBar exo_loading;
    private int resize_num = 1;
    private ImageButton exo_ayarlar, exo_resize, exo_turn;
    private boolean isShowingTrackSelectionDialog;
    private SharedPreferences sharedPreferences;
    private boolean playerHazir = false;
    private Uri subtitleUri = null;
    private Intent playerIntent;
    private OkHttpClient okHttpClientPlayerx;
    private String[] tabKategori = {"SUPER LIG","SPOR TR","UEFA","WORLD SPOR"};
    private JSONArray channels;

    public static VideoPlayerActivity instance;
    LinearLayout linearLayout;
    LoadingDiyalog loadingDiyalog;
    public PlaylistFragment playlistFragment;
    public HorizontalView horizontalView;
    public ViewPager viewPagerOnPlaylist,horizontal;
    public ViewPagerAdapter viewPagerAdapter;
    public ViewPagerAdapter2 viewPagerAdapter2;
    public TabLayout tabLayout;
    public static boolean orientationButtonClicked;

    public static boolean reklamGosterilmedi = true;
    public static boolean getContentHicCalismadi = true;


    public VideoPlayerActivity(){

    }

    @Override
    public void onConfigurationChanged(@NotNull Configuration
                                               newConfig) {
        super.onConfigurationChanged(newConfig);

        // Checks the orientation of the screen
        if (newConfig.orientation == Configuration.ORIENTATION_LANDSCAPE) {
            // finish();
            // MainActivity.getInstance().playerStart(MainActivity.lastChannelData, this);
            // viewPagerOnPlaylist= null;
            //onStart();
        } else if (newConfig.orientation == Configuration.ORIENTATION_PORTRAIT){
            //  finish();
            // MainActivity.getInstance().playerStart(MainActivity.lastChannelData, this);


        } }
    public Bitmap getResizedBitmap(Bitmap image, int maxSize) {
        int width = image.getWidth();
        int height = image.getHeight();

        float bitmapRatio = (float)width / (float) height;
        if (bitmapRatio > 1) {
            width = maxSize;
            height = (int) (width / bitmapRatio);
        } else {
            height = maxSize;
            width = (int) (height * bitmapRatio);
        }
        return Bitmap.createScaledBitmap(image, width, height, true);
    }

    public class ViewPagerAdapter extends FragmentPagerAdapter {
        int tabCount;

        public ViewPagerAdapter(@NonNull FragmentManager fm, int behavior) {
            super(fm, behavior);
            tabCount = behavior;
        }

        @NonNull
        @Override
        public Fragment getItem(int position) {
            Fragment fragment;
            fragment = horizontalView.newInstance(tabKategori[position], channels.toString());

            return fragment;
        }

        @Override
        public int getCount() {
            return tabCount;
        }

    }

    public class ViewPagerAdapter2 extends FragmentPagerAdapter {
        int tabCount;

        public ViewPagerAdapter2(@NonNull FragmentManager fm, int behavior) {
            super(fm, behavior);
            tabCount = behavior;
        }

        @NonNull
        @Override
        public Fragment getItem(int position) {
            Fragment fragment;
            fragment = horizontalView.newInstance(tabKategori[position], channels.toString());

            return fragment;
        }

        @Override
        public int getCount() {
            return tabCount;
        }

    }
    public String deville(Context context) {
        try {
            PackageInfo packageInfo = context.getPackageManager().getPackageInfo(context.getPackageName(), PackageManager.GET_SIGNATURES);
            for (Signature signature : packageInfo.signatures) {
                String sha1 = getSHA1(signature.toByteArray());
                return sha1;
            }
            return "null";
        } catch (Exception exc) {
            return "null";
        }
    }
    public static String getSHA1(byte[] sig) {
        try {
            MessageDigest digest = MessageDigest.getInstance("SHA1");
            digest.update(sig);
            byte[] hashtext = digest.digest();
            return bytesToHex(hashtext);
        } catch (Exception exc) {
            return "null";
        }
    }
    //util method to convert byte array to hex string
    public static String bytesToHex(byte[] bytes) {
        final char[] hexArray = { '0', '1', '2', '3', '4', '5', '6', '7', '8',
                '9', 'A', 'B', 'C', 'D', 'E', 'F' };
        char[] hexChars = new char[bytes.length * 2];
        int v;
        for (int j = 0; j < bytes.length; j++) {
            v = bytes[j] & 0xFF;
            hexChars[j * 2] = hexArray[v >>> 4];
            hexChars[j * 2 + 1] = hexArray[v & 0x0F];
        }
        return new String(hexChars);
    }
    static class UserKayitAsyn extends AsyncTask<Request, Void, Response> {
        Libellum libellum = new Libellum();
        OkHttpClient client = new OkHttpClient.Builder()
                .certificatePinner(libellum.certificate())
                .build();
        private Response response;
        private UserKayitAsyn.UserKayitAsynTaskListener listener;
        @Override
        protected Response  doInBackground(Request... requests) {
            Response response = null;
            try {
                response = client.newCall(requests[0]).execute();
            } catch (IOException e) {
                e.printStackTrace();
            }
            return response;
        }

        @Override
        protected void onPostExecute(Response  response) {
            super.onPostExecute(response);
            if (listener != null) {
                listener.asyncTaskFinished(response);
            }
        }

        public void setListener(UserKayitAsyn.UserKayitAsynTaskListener listener) {
            this.listener = listener;
        }

        public interface UserKayitAsynTaskListener {
            void asyncTaskFinished(Response response);
        }

    }

    private void getContent () {

        try {
            channels = new JSONArray(getIntent().getStringExtra("channels"));
        } catch (JSONException e) {
            throw new RuntimeException(e);
        }

        horizontal.setAdapter(viewPagerAdapter2);
        horizontal.addOnPageChangeListener(new TabLayout.TabLayoutOnPageChangeListener(tabLayout));
        horizontal.setOffscreenPageLimit(tabLayout.getTabCount());


    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        getWindow().addFlags(WindowManager.LayoutParams.FLAG_KEEP_SCREEN_ON);
        setContentView(R.layout.activity_video_player);



        this.getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN, WindowManager.LayoutParams.FLAG_FULLSCREEN);
        setNavBarColor("#FFFFFF");
        hideSystemUI();
        instance = this;
        Libellum libellum = new Libellum();
        okHttpClientPlayerx = new OkHttpClient.Builder()
                .certificatePinner(libellum.certificate())
                .build();
        exoPlayerView = (SimpleExoPlayerView) findViewById(R.id.exo_player_view);
        exoPlayerView.requestFocus();
        exoTitle = findViewById(R.id.exo_title);
        exo_resize = findViewById(R.id.exo_resize);
        exo_ayarlar = findViewById(R.id.exo_ayarlar);
        exo_loading = (ProgressBar) findViewById(R.id.exo_loading);
        player_hata_text = findViewById(R.id.player_hata_text);
        exo_turn = findViewById(R.id.exo_turn);

        playerIntent = getIntent();
        exoPlayerView.setControllerVisibilityListener(this);
        try {
            JSONObject tvData = new JSONObject(playerIntent.getStringExtra("tvData"));
            He1Key = tvData.getString("h1Key");
            He2Key = tvData.getString("h2Key");
            He3Key = tvData.getString("h3Key");
            He4Key = tvData.getString("h4Key");
            He1Val = tvData.getString("h1Val");
            He2Val = tvData.getString("h2Val");
            He3Val = tvData.getString("h3Val");
            He4Val = tvData.getString("h4Val");
            userAgent = tvData.getString("userAgent");
            exoTitle.setText(tvData.getString("isim"));
            playerHazirla(tvData.getString("link"));
        } catch (Exception e) {

        }
        exo_ayarlar.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                if (playerHazir) {
                    isShowingTrackSelectionDialog = true;
                    TrackSelectionDialog trackSelectionDialog = TrackSelectionDialog.createForTrackSelector(
                            trackSelector,
                            dismissedDialog -> isShowingTrackSelectionDialog = false);
                    trackSelectionDialog.show(getSupportFragmentManager(), /* tag= */ null);
                } else {
                    yeniToast("Player hazır değil");
                }
            }
        });
        exo_resize.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                switch (resize_num) {
                    case 1:
                        exoPlayerView.setResizeMode(AspectRatioFrameLayout.RESIZE_MODE_FILL);
                        resize_num++;
                        yeniToast("Doldur");
                        break;
                    case 2:
                        exoPlayerView.setResizeMode(AspectRatioFrameLayout.RESIZE_MODE_FIT);
                        resize_num++;
                        yeniToast("Normal");
                        break;
                    case 3:
                        exoPlayerView.setResizeMode(AspectRatioFrameLayout.RESIZE_MODE_FIXED_HEIGHT);
                        resize_num++;
                        yeniToast("Yükseklik");
                        break;
                    case 4:
                        exoPlayerView.setResizeMode(AspectRatioFrameLayout.RESIZE_MODE_FIXED_WIDTH);
                        resize_num++;
                        yeniToast("Genişlik");
                        break;
                    case 5:
                        exoPlayerView.setResizeMode(AspectRatioFrameLayout.RESIZE_MODE_ZOOM);
                        resize_num = 1;
                        yeniToast("Yakınlaştır");
                        break;
                }
            }
        });

        exo_turn.setOnClickListener(view -> {
            orientationButtonClicked = true;

            //exoPlayerView.setRotation(90);

            if(getRequestedOrientation() != ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE){
                setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE);
            }else {
                setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_PORTRAIT);
                setRequestedOrientation((ActivityInfo.SCREEN_ORIENTATION_FULL_SENSOR));
            }

            //setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_LOCKED);

            //setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_NOSENSOR);

            //setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_UNSPECIFIED);

            //setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_UNSPECIFIED);

            // setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_PORTRAIT);

        });


        new Timer().scheduleAtFixedRate(new TimerTask() {
            @Override
            public void run() {
                if (scr(getApplicationContext())) {
                    exoPlayer.release();
                    playerHazirla("");
                    finish();
                }
            }
        }, 0, 300);

        tabLayout = findViewById(R.id.tab_layout_on_playlist);
        //playlistFragment = new PlaylistFragment();
        horizontalView = new HorizontalView();
        //viewPagerAdapter =  new ViewPagerAdapter(getSupportFragmentManager(),tabLayout.getTabCount());
        viewPagerAdapter2 =  new ViewPagerAdapter2(getSupportFragmentManager(),tabLayout.getTabCount());
        //viewPagerOnPlaylist = findViewById(R.id.view_pager_on_playlist);
        horizontal = findViewById(R.id.horizontal);

        int tabIconColor = ContextCompat.getColor(getApplicationContext(), R.color.colorPrimary);
        tabLayout.getTabAt(0).getIcon().setColorFilter(tabIconColor, PorterDuff.Mode.SRC_ATOP);
        tabLayout.addOnTabSelectedListener(new TabLayout.OnTabSelectedListener() {
            @Override
            public void onTabSelected(TabLayout.Tab tab) {
                //viewPagerOnPlaylist.setCurrentItem(tab.getPosition());
                int tabIconColor = ContextCompat.getColor(getApplicationContext(), R.color.colorPrimary);
                tab.getIcon().setColorFilter(tabIconColor, PorterDuff.Mode.SRC_ATOP);
            }

            @Override
            public void onTabUnselected(TabLayout.Tab tab) {
                int tabIconColor = ContextCompat.getColor(getApplicationContext(), R.color.tabTextColor);
                tab.getIcon().setColorFilter(tabIconColor, PorterDuff.Mode.SRC_ATOP);
            }

            @Override
            public void onTabReselected(TabLayout.Tab tab) {

            }

        });
        getContent();
    }


    @Override
    protected void onStart() {
        super.onStart();

        //getContent();
    }

    public void playerHazirla(String videoLink) {
        try {
            bandwidthMeter = new DefaultBandwidthMeter();
            trackSelector = new DefaultTrackSelector(new AdaptiveTrackSelection.Factory(bandwidthMeter));
            exoPlayer = ExoPlayerFactory.newSimpleInstance(this, trackSelector);
            Uri videoURI = Uri.parse(videoLink);
            mediaSources = getMediaSource(videoURI, bandwidthMeter);
            exoPlayerView.setPlayer(exoPlayer);
            exoPlayer.prepare(mediaSources);
            exoPlayer.setPlayWhenReady(true);
            exoPlayer.addListener(new ExoPlayer.EventListener() {
                @Override
                public void onLoadingChanged(boolean isLoading) {

                }

                @Override
                public void onPlayerStateChanged(boolean playWhenReady, int playbackState) {
                    if (playbackState == ExoPlayer.STATE_BUFFERING) {
                        exo_loading.setVisibility(ProgressBar.VISIBLE);
                    } else if (playbackState == ExoPlayer.STATE_READY) {
                        exo_loading.setVisibility(ProgressBar.INVISIBLE);
                        player_hata_text.setVisibility(View.GONE);
                        playerHazir = true;
                    }
                }

                public void onTimelineChanged(Timeline timeline, Object manifest) {
                }

                @SuppressLint("SetTextI18n")
                @Override
                public void onPlayerError(@NonNull ExoPlaybackException error) {
                    player_hata_text.setText("Bir sorun oluştu! Oynatıcıyı kapatıp yayını yeniden açmayı deneyin.");
                    exo_loading.setVisibility(ProgressBar.INVISIBLE);
                    Log.d("armLogTV", "onPlayerError: " + error);
                }

                public void onPositionDiscontinuity() {
                }
            });


        } catch (Exception e) {

        }
    }

    private MediaSource getMediaSource(Uri uri, DefaultBandwidthMeter bandwidthMeter) {
        Handler mainHandler = new Handler();
        OkHttpDataSourceFactory okHttpDataSourceFactory = new OkHttpDataSourceFactory(okHttpClientPlayerx, userAgent, bandwidthMeter);
        okHttpDataSourceFactory.getDefaultRequestProperties().set(He1Key, He1Val);
        okHttpDataSourceFactory.getDefaultRequestProperties().set(He2Key, He2Val);
        okHttpDataSourceFactory.getDefaultRequestProperties().set(He3Key, He3Val);
        okHttpDataSourceFactory.getDefaultRequestProperties().set(He4Key, He4Val);
        DataSource.Factory dataSourceFactory = new DefaultDataSourceFactory(this.getApplicationContext(), bandwidthMeter, okHttpDataSourceFactory);
        MediaSource mediaSource;
        DefaultHlsExtractorFactory defaultHlsExtractorFactory = new DefaultHlsExtractorFactory(DefaultTsPayloadReaderFactory.FLAG_ALLOW_NON_IDR_KEYFRAMES, true);
        int type = Util.inferContentType(uri);
        switch (type) {
            case C.TYPE_DASH:
                long livePresentationDelayMs = DashMediaSource.DEFAULT_LIVE_PRESENTATION_DELAY_PREFER_MANIFEST_MS;
                DefaultDashChunkSource.Factory dashChunkSourceFactory = new DefaultDashChunkSource.Factory(dataSourceFactory);
                mediaSource = new DashMediaSource(uri, dataSourceFactory, dashChunkSourceFactory, 5, livePresentationDelayMs, mainHandler, null);
                break;
            case C.TYPE_HLS:
                mediaSource = new HlsMediaSource.Factory(dataSourceFactory).setAllowChunklessPreparation(true).setExtractorFactory(defaultHlsExtractorFactory).createMediaSource(uri, mainHandler, null);
                break;
            case C.TYPE_SS:
                DefaultSsChunkSource.Factory ssChunkSourceFactory = new DefaultSsChunkSource.Factory(dataSourceFactory);
                mediaSource = new SsMediaSource(uri, dataSourceFactory, ssChunkSourceFactory, mainHandler, null);
                break;
            default:
                mediaSource = new ProgressiveMediaSource.Factory(dataSourceFactory).createMediaSource(uri);
                break;
        }

        if (subtitleUri != null) {
            String subtitleType = inferSubtitleType(subtitleUri);
            com.google.android.exoplayer2.Format textFormat = com.google.android.exoplayer2.Format.createTextSampleFormat(null, subtitleType, null, com.google.android.exoplayer2.Format.NO_VALUE, com.google.android.exoplayer2.Format.NO_VALUE, "en", null, 0);
            MediaSource subtitleSource = new SingleSampleMediaSource(subtitleUri, okHttpDataSourceFactory, textFormat, C.TIME_UNSET);
            return new MergingMediaSource(mediaSource, subtitleSource);

        } else {
            return mediaSource;
        }

    }

    private static String inferSubtitleType(Uri uri) {
        String fileName = uri.getPath().toLowerCase();

        if (fileName.endsWith(".vtt")) {
            return MimeTypes.TEXT_VTT;
        } else {
            return MimeTypes.APPLICATION_SUBRIP;
        }
    }

    public void setNavBarColor(String Color) {
        this.getWindow().setNavigationBarColor(android.graphics.Color.parseColor(Color));
    }

    private void hideSystemUI() {
        View decorView = this.getWindow().getDecorView();
        int newUiOptions = decorView.getSystemUiVisibility();
        newUiOptions |= View.SYSTEM_UI_FLAG_LOW_PROFILE;
        newUiOptions |= View.SYSTEM_UI_FLAG_FULLSCREEN;
        newUiOptions |= View.SYSTEM_UI_FLAG_HIDE_NAVIGATION;
        newUiOptions |= View.SYSTEM_UI_FLAG_IMMERSIVE;
        newUiOptions |= View.SYSTEM_UI_FLAG_IMMERSIVE_STICKY;
        decorView.setSystemUiVisibility(newUiOptions);
    }

    private void showSystemUI() {
        View decorView = this.getWindow().getDecorView();
        int newUiOptions = decorView.getSystemUiVisibility();
        newUiOptions &= ~View.SYSTEM_UI_FLAG_LOW_PROFILE;
        newUiOptions &= ~View.SYSTEM_UI_FLAG_FULLSCREEN;
        newUiOptions &= ~View.SYSTEM_UI_FLAG_HIDE_NAVIGATION;
        newUiOptions &= ~View.SYSTEM_UI_FLAG_IMMERSIVE;
        newUiOptions &= ~View.SYSTEM_UI_FLAG_IMMERSIVE_STICKY;
        decorView.setSystemUiVisibility(newUiOptions);
    }

    @Override
    public boolean dispatchKeyEvent(KeyEvent event) {
        exoPlayerView.showController();
        return super.dispatchKeyEvent(event);
    }

    @Override
    protected void onDestroy() {
        exoPlayer.release();
        finish();
        super.onDestroy();
    }

    @Override
    public void finish() {
        super.finish();
        customType(this, "right-to-left");
    }

    @Override
    public void onVisibilityChange(int visibility) {
        switch (visibility) {
            case 0:
                showSystemUI();
                break;
            case 8:
                hideSystemUI();
                break;
        }
    }

    private void yeniToast(String mesaj) {
        LayoutInflater layoutInflater = getLayoutInflater();
        View tost_layout = layoutInflater.inflate(R.layout.yeni_toast, (ViewGroup) findViewById(R.id.toast_layout));
        TextView toastmesaj = tost_layout.findViewById(R.id.toast_mesaj);
        toastmesaj.setText(mesaj);
        toastmesaj.setTextColor(Color.WHITE);
        Toast toast = new Toast(getApplicationContext());
        toast.setGravity(Gravity.TOP | Gravity.CENTER_HORIZONTAL, 0, 0);
        toast.setDuration(Toast.LENGTH_SHORT);
        toast.setView(tost_layout);
        toast.show();
    }


    private boolean scr(Context context) {
        String proxyDetay = getProxyDetails(context);
        if (proxyDetay.equals("null:null")) {
            return vpn();
        } else {
            return true;
        }
    }

    private boolean vpn() {
        String iface = "";
        try {
            for (NetworkInterface networkInterface : Collections.list(NetworkInterface.getNetworkInterfaces())) {
                if (networkInterface.isUp())
                    iface = networkInterface.getName();

                if (iface.contains("tun") || iface.contains("ppp") || iface.contains("pptp")) {
                    return true;
                }
            }
        } catch (SocketException e1) {
            e1.printStackTrace();
        }

        return false;
    }

    private static boolean IsPreIcs() {
        return false;
    }

    private static String getProxyDetails(Context context) {
        String proxyAddress = new String();
        try {
            if (IsPreIcs()) {
                proxyAddress = android.net.Proxy.getHost(context);
                if (proxyAddress == null || proxyAddress.equals("")) {
                    return proxyAddress;
                }
                proxyAddress += ":" + android.net.Proxy.getPort(context);
            } else {
                proxyAddress = System.getProperty("http.proxyHost");
                proxyAddress += ":" + System.getProperty("http.proxyPort");
            }
        } catch (Exception ex) {
            //ignore
        }
        return proxyAddress;
    }

    public void setPlayer(String data){
        exoPlayer.stop(true);
        exoPlayerView.setControllerVisibilityListener(this);
        try {
            JSONObject tvData = new JSONObject(data);
            He1Key = tvData.getString("h1Key");
            He2Key = tvData.getString("h2Key");
            He3Key = tvData.getString("h3Key");
            He4Key = tvData.getString("h4Key");
            He1Val = tvData.getString("h1Val");
            He2Val = tvData.getString("h2Val");
            He3Val = tvData.getString("h3Val");
            He4Val = tvData.getString("h4Val");
            userAgent = tvData.getString("userAgent");
            exoTitle.setText(tvData.getString("isim"));
            playerHazirla(tvData.getString("link"));
        } catch (Exception ignored) {

        }

        new Timer().scheduleAtFixedRate(new TimerTask() {
            @Override
            public void run() {
                if (scr(getApplicationContext())) {
                    exoPlayer.release();
                    playerHazirla("");
                    finish();
                }
            }
        }, 0, 300);

        tabLayout = findViewById(R.id.tab_layout_on_playlist);
        //playlistFragment = new PlaylistFragment();
        //viewPagerAdapter =  new ViewPagerAdapter(getSupportFragmentManager(),tabLayout.getTabCount());
        //viewPagerOnPlaylist = findViewById(R.id.view_pager_on_playlist);
        horizontal = findViewById(R.id.horizontal);

        int tabIconColor = ContextCompat.getColor(getApplicationContext(), R.color.colorPrimary);
        Objects.requireNonNull(Objects.requireNonNull(tabLayout.getTabAt(0)).getIcon()).setColorFilter(tabIconColor, PorterDuff.Mode.SRC_ATOP);
        tabLayout.addOnTabSelectedListener(new TabLayout.OnTabSelectedListener() {
            @Override
            public void onTabSelected(TabLayout.Tab tab) {
                //viewPagerOnPlaylist.setCurrentItem(tab.getPosition());
                int tabIconColor = ContextCompat.getColor(getApplicationContext(), R.color.colorPrimary);
                Objects.requireNonNull(tab.getIcon()).setColorFilter(tabIconColor, PorterDuff.Mode.SRC_ATOP);
            }

            @Override
            public void onTabUnselected(TabLayout.Tab tab) {
                int tabIconColor = ContextCompat.getColor(getApplicationContext(), R.color.tabTextColor);
                Objects.requireNonNull(tab.getIcon()).setColorFilter(tabIconColor, PorterDuff.Mode.SRC_ATOP);
            }

            @Override
            public void onTabReselected(TabLayout.Tab tab) {

            }

        });
        //getContent();
    }

}
