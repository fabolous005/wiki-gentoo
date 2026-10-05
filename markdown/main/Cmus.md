<!-- source: https://wiki.gentoo.org/wiki/Cmus | group: Gentoo Wiki (Main) | wiki-title: Cmus -->
---
title: cmus
url: https://wiki.gentoo.org/wiki/Cmus
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-10-04"
fingerprint: e400d87a5b869f92
license: CC BY-SA 4.0
---

# cmus

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**cmus** is a small, fast, and powerful console music player for Unix-like operating systems.

## Installation

### USE flags


| [+flac](https://packages.gentoo.org/useflags/+flac) | Add support for FLAC: Free Lossless Audio Codec | 
| [+mad](https://packages.gentoo.org/useflags/+mad) | Add support for mad (high-quality mp3 decoder library and cli frontend) | 
| [+unicode](https://packages.gentoo.org/useflags/+unicode) | Add support for Unicode | 
| [+vorbis](https://packages.gentoo.org/useflags/+vorbis) | Add support for the OggVorbis audio codec | 
| [aac](https://packages.gentoo.org/useflags/aac) | Enable support for MPEG-4 AAC Audio | 
| [alsa](https://packages.gentoo.org/useflags/alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [ao](https://packages.gentoo.org/useflags/ao) | Use libao audio output library for sound playback | 
| [cddb](https://packages.gentoo.org/useflags/cddb) | Access cddb servers to retrieve and submit information about compact disks | 
| [cdio](https://packages.gentoo.org/useflags/cdio) | Use libcdio for CD support | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [discid](https://packages.gentoo.org/useflags/discid) | Enable reading the ID of the inserted CD | 
| [elogind](https://packages.gentoo.org/useflags/elogind) | Enable MPRIS support via sys-auth/elogind | 
| [examples](https://packages.gentoo.org/useflags/examples) | Install examples, usually source code | 
| [ffmpeg](https://packages.gentoo.org/useflags/ffmpeg) | Enable ffmpeg/libav-based audio/video codec support | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [libsamplerate](https://packages.gentoo.org/useflags/libsamplerate) | Build with support for converting sample rates using libsamplerate | 
| [mikmod](https://packages.gentoo.org/useflags/mikmod) | Add libmikmod support to allow playing of SoundTracker-style music files | 
| [modplug](https://packages.gentoo.org/useflags/modplug) | Add libmodplug support for playing SoundTracker-style music files | 
| [mp4](https://packages.gentoo.org/useflags/mp4) | Support for MP4 container format | 
| [musepack](https://packages.gentoo.org/useflags/musepack) | Enable support for the musepack audio codec | 
| [opus](https://packages.gentoo.org/useflags/opus) | Enable Opus audio codec support | 
| [oss](https://packages.gentoo.org/useflags/oss) | Add support for OSS (Open Sound System) | 
| [pidgin](https://packages.gentoo.org/useflags/pidgin) | Install support script for net-im/pidgin | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [sndio](https://packages.gentoo.org/useflags/sndio) | Add support for media-sound/sndio | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable MPRIS support via sys-apps/systemd | 
| [tremor](https://packages.gentoo.org/useflags/tremor) | Use libivorbis from media-libs/tremor instead of media-libs/libvorbis | 
| [wavpack](https://packages.gentoo.org/useflags/wavpack) | Add support for wavpack audio compression tools | 

### Emerge

`root #``emerge --ask media-sound/cmus`
## Usage

### Starting Cmus

When launching cmus (just type cmus in a terminal and press Enter) it will open to the album/artist view, which looks something like this:

+---------------------------------------------------------------------+
| Artist / Album             Track                            Library |
|                          |                                          |
|                          |                                          |
|                          |                                          |
|                          |                                          |
|                          |                                          |
|                          |                                          |
|                          |                                          |
|                                                                     |
| . 00:00 - 00:00 vol: 100                     all from library | C   |
|                                                                     |
+---------------------------------------------------------------------+

### Adding Music

To add music to the cmus library, press `5` to switch to the file-browser view.

### Playing Tracks From The Library

To start playing tracks from the library, press `2` to go to the simple library view.

+---------------------------------------------------------------------+
| Artist / Album            Track                              Library|
| The Beatles              | Come Together                     04:18  |
|                          | Something                         03:02  |
|                          | Maxwell's Silver Hammer           03:27  |
|                          | Oh! Darling                       03:26  |
|                          | Octopus's Garden                  02:51  |
|                          | Here Comes The Sun                03:05  |
|                          | Because                           02:45  |
|                          | You Never Give Me Your Money      04:02  |
|                          | Sun King                          02:26  |
| . 00:00 - 47:49 vol: 100                     all from library | C   |
+---------------------------------------------------------------------+

To control the playback, the following keys can be used:

- Press `C` to pause/unpause.
- Press `→`/`←` to seek by 10 seconds.

### Settings

Cmus offers a variety of customizable settings. Press `7` to go to the settings menu where settings can be changed, new keys bound, and colors changed.

+---------------------------------------------------------------------+
| Settings                                                            |
| aaa\_mode                                 all                        |
| altformat\_current                        %F                         |
| altformat\_playlist                       %f%= %d                    |
| altformat\_title                          %f                         |
| ltformat\_trackwin                        %f%= %d                    |
| auto\_expand\_albums\_follow                true                       |
|                                                                     |
| . 00:00 - 2:16:25 vol: 100                                          |
|                                                                     |
+---------------------------------------------------------------------+

## Keys

### Changing views

- Press `1` for the library view.
- Press `3` for the playlist view.
- Press `4` for the play queue view.
- Press `6` for the live filter view.
- Press `8` for the browser view.

- Press `J` or `↓` to move down.
- Press `K` or `↑` to move up.
- Press `L` or `→` to enter directory or play selected track.
- Press `H` or `←` to navigate to the parent directory.
- Press `ENTER` to add selected track or all tracks in selected directory to play queue.

### Volume control

- Press `-` to lower the volume.
- Press `+` to raise the volume.

### Searching

- Press `/` followed by the search term and `ENTER` to search.
- Press `n` to move to the next match.
- Press `N` to move to the previous match.

### Playback control

- Press `B` to play the next track in the playlist.
- Press `Z` to play the previous track in the playlist.
- Press `X` to stop playing.
- Press `V` to show/hide the status window.
- Press `SPACE` to toggle play/pause.

### Managing playlists and queues

- Press `U` to update the database.
- Press `E` to clear the play queue.
- Press `SHIFT`+`Y` to add all tracks in the library view to the play queue.
- Press `Y` to add all tracks in the current view to the play queue.

## Configuration

Documented default settings for cmus with detailed descriptions.

For alternative values, please refer to the cmus manual. Additionally, provided a comprehensive table showcasing all available settings.

| Config Option | Value | Description | 
|---|---|---|
| aaa\_mode | all | Sets the behavior for the 'a' key. 'all' plays the current track and adds the whole album to the playlist. | 
| altformat\_current | %F | Specifies the alternative format for the current track. | 
| altformat\_playlist | %f%= %d | Sets the alternative format for the playlist view. | 
| altformat\_title | %f | Specifies the alternative format for the title line. | 
| altformat\_trackwin | %f%= %d | Sets the alternative format for the track window. | 
| auto\_expand\_albums\_follow | true | Automatically expands albums when using the 'follow' command. | 
| auto\_expand\_albums\_search | true | Automatically expands albums when searching. | 
| auto\_expand\_albums\_selcur | true | Automatically expands albums when selecting the current track. | 
| auto\_reshuffle | true | Automatically reshuffles the playlist after reaching the end. | 
| buffer\_seconds | 10 | Specifies the number of seconds to buffer for gapless playback. | 
| fset 90s | date>=1990&date\<2000 | Defines a filter set named '90s' to match tracks with a date between 1990 and 1999. | 
| fset classical | genre="Classical" | Defines a filter set named 'classical' to match tracks with the genre "Classical". | 
| fset missing-tag | album=""\|title=""\|tracknumber=-1\|date=-1) | Defines a filter set named 'missing-tag' to match tracks with missing tags. | 
| fset mp3 | filename="\*.mp3" | Defines a filter set named 'mp3' to match tracks with the .mp3 file extension. | 
| fset ogg | filename="\*.ogg" | Defines a filter set named 'ogg' to match tracks with the .ogg file extension. | 
| fset ogg-or-mp3 | mp3 | Defines a filter set named 'ogg-or-mp3' to match tracks with either the .ogg or .mp3 file extension. | 
| fset unheard | play\_count=0 | Defines a filter set named 'unheard' to match tracks with a play count of 0. | 
| factivate |  | Activates the last defined filter set. | 
| input.aac.priority | 50 | Sets the priority for AAC audio files. | 
| input.cue.priority | 50 | Sets the priority for CUE sheet files. | 
| input.flac.priority | 50 | Sets the priority for FLAC audio files. | 
| input.mad.priority | 55 | Sets the priority for MP3 audio files using the MAD plugin. | 
| input.mp4.priority | 50 | Sets the priority for MP4 audio files. | 
| input.vorbis.priority | 50 | Sets the priority for Ogg Vorbis audio files. | 
| input.wav.priority | 50 | Sets the priority for WAV audio files. | 
| lib\_add\_filter |  | Specifies additional filtering rules for the library. | 
| lib\_sort | Sets the sorting rules for the library view. |  | 
| mixer.alsa.channel |  | Specifies the ALSA mixer channel to use for volume control. | 
| mixer.alsa.device |  | Specifies the ALSA mixer device to use for volume control. | 
| mixer.pulse.restore\_volume | 1 | Specifies whether to restore the volume level when restarting cmus with the PulseAudio output plugin. | 
| mouse | true | Enables mouse support. | 
| mpris | true | Enables support for the MPRIS D-Bus interface. | 
| output\_plugin | pulse | Sets the output plugin for audio playback. | 
| passwd |  | Sets the password for connecting to password-protected streams. | 
| pause\_on\_output\_change | false | Specifies whether to pause playback when changing the output plugin. | 
| pl\_sort |  | Specifies the sorting rules for the playlist view. | 
| play\_library | true | Enables playback from the library. | 
| play\_sorted | false | Enables playback from the sorted view. | 
| repeat | false | Enables repeat mode. | 
| repeat\_current | false | Enables repeat current track mode. | 
| replaygain | disabled | Sets the replay gain mode. | 
| replaygain\_limit | true | Specifies whether to apply the replay gain limit. | 
| replaygain\_preamp | 0.000000 | Sets the replay gain pre-amplification level. | 
| resume | false | Enables resume playback from the last position. | 
| rewind\_offset | 5 | Sets the rewind offset in seconds. | 
| scroll\_offset | 2 | Sets the scroll offset for lists and windows. | 
| set\_term\_title | true | Sets the terminal window title. | 
| show\_all\_tracks | true | Specifies whether to show all tracks in the library view. | 
| show\_current\_bitrate | false | Specifies whether to display the current bitrate in the status line. | 
| show\_hidden | false | Specifies whether to show hidden files and directories. | 
| show\_playback\_position | true | Specifies whether to display the playback position in the status line. | 
| show\_remaining\_time | false | Specifies whether to display the remaining time in the status line. | 
| shuffle | off | Sets the shuffle mode. | 
| skip\_track\_info | false | Specifies whether to skip track information when available. | 
| smart\_artist\_sort | true | Enables smart sorting for artist names. | 
| softvol | false | Enables software volume control. | 
| softvol\_state | 0 0 | Sets the initial software volume control state. | 
| start\_view | tree | Sets the initial view when starting cmus. | 
| status\_display\_program |  | Specifies a custom program to display the status line. | 
| stop\_after\_queue | false | Specifies whether to stop playback after the queue has been played. | 
| time\_show\_leading\_zero | true | Specifies whether to show leading zeros in time displays. | 
| tree\_width\_max | 0 | Sets the maximum width of the tree view. | 
| tree\_width\_percent | 33 | Sets the width percentage for the tree view. | 
| wrap\_search | true | Specifies whether to wrap around when searching. | 
| device | /dev/cdrom | Sets the device to use for CD playback. | 
| display\_artist\_sort\_name | false | Specifies whether to display the artist's sort name instead of the artist name. | 
| dsp.alsa.device |  | Specifies the ALSA device to use for audio output. | 
| follow | false | Sets the behavior for the 'f' key. 'false' disables auto-following. | 
| icecast\_default\_charset | ISO-8859-1 | Sets the default character set for Icecast streams. | 
| id3\_default\_charset | ISO-8859-1 | Sets the default character set for ID3 tags. | 
| confirm\_run | true | Enables confirmation prompt before executing shell commands. | 
| continue | true | Enables continuous playback when reaching the end of the playlist. | 
| continue\_album | true | Enables continuous playback when reaching the end of an album. | 

## Colors

In Cmus, a custom theme can be specified by 'theme\_name' or by editing the colors directly in the \~/.config/cmus/autosave file. Alternatively, the `7` key can be pressed in the player to open the settings, and if the mouse is enabled, right-click to generate the setting in the command line. Alternatively, navigate to the option in the list and press Enter. For certain settings, press the `tab` key after selecting the option and delete the current value to display alternatives.

Below are all the possible options with explanations. Colors can be selected by their names, such as 'red', or by the corresponding ANSI color codes from 0 to 256.

- 0: Black
- 1: Red
- 2: Green
- 3: Yellow
- 4: Blue
- 5: Magenta
- 6: Cyan
- 7: White

When using ANSI color codes ranging from 1 to 256 to specify colors, it can be helpful to display a list of possible colors. To display a list, copy and paste the following code into the terminal:

| Configuration options |  |  | 
|---|---|---|
| Config option | Value | Description | 
|---|---|---|
| color\_cmdline\_attr | default | Sets the attribute color for the command line. | 
| color\_cmdline\_bg | default | Sets the background color for the command line. | 
| color\_cmdline\_fg | default | Sets the foreground color for the command line. | 
| color\_cur\_sel\_attr | default | Sets the attribute color for the currently selected item. | 
| color\_error | lightred | Sets the color for error messages. | 
| color\_info | lightyellow | Sets the color for informational messages. | 
| color\_separator | blue | Sets the color for separators. | 
| color\_statusline\_attr | default | Sets the attribute color for the status line. | 
| color\_statusline\_bg | gray | Sets the background color for the status line. | 
| color\_statusline\_fg | black | Sets the foreground color for the status line. | 
| color\_titleline\_attr | default | Sets the attribute color for the title line. | 
| color\_titleline\_bg | blue | Sets the background color for the title line. | 
| color\_titleline\_fg | white | Sets the foreground color for the title line. | 
| color\_trackwin\_album\_attr | bold | Sets the attribute color for the album name in the track window. | 
| color\_trackwin\_album\_bg | default | Sets the background color for the album name in the track window. | 
| color\_trackwin\_album\_fg | default | Sets the foreground color for the album name in the track window. | 
| color\_win\_attr | default | Sets the attribute color for the windows. | 
| color\_win\_bg | default | Sets the background color for the windows. | 
| color\_win\_cur | lightyellow | Sets the color for the currently selected item in the windows. | 
| color\_win\_cur\_attr | default | Sets the attribute color for the currently selected item in the windows. | 
| color\_win\_cur\_sel\_attr | default | Sets the attribute color for the currently selected and playing item in the windows. | 
| color\_win\_cur\_sel\_bg | blue | Sets the background color for the currently selected and playing item in the windows. | 
| color\_win\_cur\_sel\_fg | lightyellow | Sets the foreground color for the currently selected and playing item in the windows. | 
| color\_win\_dir | lightblue | Sets the color for directories in the windows. | 
| color\_win\_fg | default | Sets the foreground color for the windows. | 
| color\_win\_inactive\_cur\_sel\_attr | default | Sets the attribute color for the inactive currently selected and playing item in the windows. | 
| color\_win\_inactive\_cur\_sel\_bg | gray | Sets the background color for the inactive currently selected and playing item in the windows. | 
| color\_win\_inactive\_cur\_sel\_fg | lightyellow | Sets the foreground color for the inactive currently selected and playing item in the windows. | 
| color\_win\_inactive\_sel\_attr | default | Sets the attribute color for the inactive selected item in the windows. | 
| color\_win\_inactive\_sel\_bg | gray | Sets the background color for the inactive selected item in the windows. | 
| color\_win\_inactive\_sel\_fg | black | Sets the foreground color for the inactive selected item in the windows. | 
| color\_win\_sel\_attr | default | Sets the attribute color for the selected item in the windows. | 
| color\_win\_sel\_bg | blue | Sets the background color for the selected item in the windows. | 
| color\_win\_sel\_fg | white | Sets the foreground color for the selected item in the windows. | 
| color\_win\_title\_attr | default | Sets the attribute color for the window titles. | 
| color\_win\_title\_bg | blue | Sets the background color for the window titles. | 
| color\_win\_title\_fg | white | Sets the foreground color for the window titles. | 

### The playlist

The playlist works like another library (like view 2) except that (like the queue) you manually set the order of the tracks. This can be quite useful when creating a mix of specific tracks or to listen to an audio book without having the chapters when playing "all from library".

The playlist is on view 3. But before we go there, let's add some tracks. Press `2` to go to the simple library view, go to a desired track and press `Y` to add it to the playlist. The visual feedback will present the highlighting of the track and the cursor will move down one row. Select a few more tracks for a working list.

Now press `3` to go to the playlist.

Just like the queue, use the `P`, `SHIFT`+`P`, and `SHIFT`+`D` keys to move and delete tracks from the playlist.

### Search

Press `2` to be sure the simple library view is shown, then press `/` to start a search. Type a word or two from the track to be queried. cmus will search for all tracks that have all those words in their titles. Press `ENTER` to get the keyboard out of the search command, and `N` to find the next match.

### Tree view

Press `1` to select the tree view. Scroll to the artist, press `SPACE` to show their albums, scroll to the desired album, then press `TAB` so the keyboard controls the right column. Press `TAB` again to get back to the left column.

### Quit

When done, type :q and press Enter to quit. This will save the settings, library, playlist, and queue.

## See also

- [VLC](https://wiki.gentoo.org/wiki/VLC) — a wildly popular, cross platform video player and streamer.
