Waveforms Exporter
=================

Forked from
[Waveform Playlist](http://doge.mit-license.org)

Export numerous audio tracks to .WAV format.

###Example Usage:

    componentDidMount() {
        var playlist = WaveformPlaylist.init({
            samplesPerPixel: 3000,
            waveHeight: 100,
            container: document.getElementById("tracks-container"),
            state: 'cursor',
            colors: {
                waveOutlineColor: '#E0EFF1',
                timeColor: 'grey',
                fadeColor: 'black'
            },
            timescale: false,
            controls: {
                show: false, //whether or not to include the track controls
                width: 0 //width of controls in pixels
            },
            seekStyle : 'line',
            zoomLevels: [500, 1000, 3000, 5000]
        });

        playlist.load([
            {
                "src": this.props.audioFile.url(),
                "name": "Vocals",
                "fadeIn": {
                    "duration": 0.5
                },
                "fadeOut": {
                    "duration": 0.5
                },
                "cuein": 0,
                "cueout": 14.5
            },
        ]).then(function() {
            playlist.initExporter();
            var downloadUrl = undefined;
            ee.emit('startaudiorendering', 'wav');
            ee.on('audiorenderingfinished', function (type, data) {
                if (type == 'wav'){
                    if (downloadUrl) {
                        window.URL.revokeObjectURL(downloadUrl);
                    }

                    downloadUrl = window.URL.createObjectURL(data);
                    console.log(downloadUrl, 'huhurl')

                    displayDownloadLink(downloadUrl);
                }
            });
        });

        var ee = playlist.getEventEmitter();


    }

    // Outside the Component scope
    function displayDownloadLink(link) {
    let a = document.createElement("a");
    document.body.appendChild(a);
    a.style = "display: none";
        a.href = link;
        a.download = 'test.wav';
        a.click();
        window.URL.revokeObjectURL(link);
}





##TODO
Support exporting audio files with DSP alterations (like EQ, Reverb).

## License

[MIT License](http://doge.mit-license.org)
