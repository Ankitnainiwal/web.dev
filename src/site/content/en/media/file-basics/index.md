---
layout: post
title: File basics
authors:
  - joemedley
description: |
  You might think that you can take a video camera and upload it to the web as
  is. Preparing a video for serving from your own site is a bit more
  complicated.
date: 2020-06-19
updated: 2020-08-20
tags:
  - FFmpeg
  - files
  - Shaka
---

You might think that you can take a video camera and upload it to the web as is.
Indeed, video streaming sites such as YouTube or Vimeo let you do this. These
sites simplify video processing and uploading for the sake of customer
service. Preparing a video for serving from your own site is a bit more
complicated.

Video files come in a variety of formats. The format that comes off your camera,
typically a mov file, is good for recording and for editing and other early
post-production processes. Its size means it's not good for streaming over the
web. Because browsers support different file formats, you'll need to create
multiple files. Before converting files you need to understand a few basics
about them and about their characteristics.

The file that you see in your operating system shell is a _container_,
identified by a file extension (mp4, webm, etc.). The container houses one or
more _streams_. A media file can have any number of streams, of [more
types](https://developer.mozilla.org/en-US/docs/Web/Media/Formats) than I will
go into here.

The sample files used later in this section contain at most two streams: an
audio stream and a video stream. Among the other types you might encounter are
captions and data, both of which are beyond the scope of this article. There are
instances where audio and video streams are dealt with separately. Most files
you'll encounter will only contain a single audio stream and a single video
stream.

Within the audio and video streams, the actual data is compressed using a codec.
A _codec_, or coder/decoder, is a compression format for video or audio data. The
distinction between a container and a codec is important becasue files with the
same container can have their contents encoded with different codecs.

The image below illustrates this structure. On the left is the basic structure.
On the right are the specifics of that structure for a single webm file.

<figure class="w-figure  w-figure--inline-right">
  <img src="./media-container-onion.png" alt="Comparing media file structure with a hypothetical media file.">
  <figcaption class="w-figcaption">Parts of a media file.</figcaption>
</figure>

Not all browsers support up-to-date containers and codecs. For example, webm is
high-quality and open source. Files in webm packages can be orders of magnitude
smaller than other formats, making them a good choice for mobile download.
Although webm was created specifically for the web, its support is not yet
universal. Safari in particular does not, as of this writing, support webm for
embedded video.

All modern browsers support mp4 files, making them a good general choice for a
media container and the best choice as the backup container for webm.

Many file formats support multiple codecs for the same stream type. A complete
list of available [video
codecs](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Video_codecs)
and [audio
codecs](ttps://developer.mozilla.org/en-US/docs/Web/Media/Formats/Audio_codecs)
would be a whole website itself. The links just provided are for MDN's practical
lists of what's usable on the web. Listed below are the currently preferred file
types and the codecs they may use. Follow the links for browser support lists.

| File type | Video Codec | Audio Codec |
| ---- | ----- | ---- |
| [mp4](https://caniuse.com/#search=mp4)  | [AV1](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Video_codecs#AV1), [AVC (H.264)](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Video_codecs#AVC_H.264)*, [VP9](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Video_codecs#VP9) | [aac](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Audio_codecs#AAC) |
| [webm](https://caniuse.com/#feat=webm) | [AV1](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Video_codecs#AV1), [VP9](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Video_codecs#VP9)* | [vorbis](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Audio_codecs#Vorbis), [opus](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Audio_codecs#Opus) |
&#42; Indicates the the preferred video codec.

_Bitrate_ is the maximum number of bits used to encode one second of a stream.
The more bits used to encode a second of stream, the higher the potential
detail and fidelity.

_Resolution_ is the amount of information in a single frame of video, given as
the number of logical pixels in each dimension. I provide more information about this concept [later in this series of lessons](../resolution).

In the next section, you'll examine these characteristics using two command line
tools: Shaka Packager and FFmpeg.