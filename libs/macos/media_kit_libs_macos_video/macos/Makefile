all: Frameworks/*.xcframework Frameworks/.symlinks

MPV_XCFRAMEWORKS_VERSION=v0.6.2
MPV_XCFRAMEWORKS_SHA256SUM=73e147273a6c7a19bd5535ab2336188d12636caa0d65422f86a5e1ac853747fd
# MPV_XCFRAMEWORKS_SHA256SUM=20c5add312f4cdb2a9cbf589fcf4495973575db3d90a140747c331690156364e
# MPV_XCFRAMEWORKS_SHA256SUM=84d2ad98e046e82c6dc34d8547d76c2afeaee89c0f53032773be8985c95536d6

.cache/xcframeworks/libmpv-xcframeworks-${MPV_XCFRAMEWORKS_VERSION}-macos-universal.tar.gz:
	mkdir -p .cache/xcframeworks
	rm -f .cache/xcframeworks/*.tmp .cache/xcframeworks/*-macos-universal.tar.gz
	curl -L \
		https://github.com/media-kit/libmpv-darwin-build/releases/download/${MPV_XCFRAMEWORKS_VERSION}/libmpv-xcframeworks_${MPV_XCFRAMEWORKS_VERSION}_macos-universal-video-full.tar.gz \
		-o .cache/xcframeworks/libmpv.tar.gz.tmp
	shasum -a 256 -c <<< '${MPV_XCFRAMEWORKS_SHA256SUM}  .cache/xcframeworks/libmpv.tar.gz.tmp'
	mv .cache/xcframeworks/libmpv.tar.gz.tmp .cache/xcframeworks/libmpv-xcframeworks-${MPV_XCFRAMEWORKS_VERSION}-macos-universal.tar.gz
	touch .cache/xcframeworks/libmpv-xcframeworks-${MPV_XCFRAMEWORKS_VERSION}-macos-universal.tar.gz

.cache/xcframeworks/libmpv-xcframeworks-macos-universal.tar.gz: .cache/xcframeworks/libmpv-xcframeworks-${MPV_XCFRAMEWORKS_VERSION}-macos-universal.tar.gz
	rm -f .cache/xcframeworks/libmpv-xcframeworks-macos-universal.tar.gz
	ln -s libmpv-xcframeworks-${MPV_XCFRAMEWORKS_VERSION}-macos-universal.tar.gz .cache/xcframeworks/libmpv-xcframeworks-macos-universal.tar.gz

Frameworks/*.xcframework: .cache/xcframeworks/libmpv-xcframeworks-macos-universal.tar.gz
	mkdir -p Frameworks
	rm -rf Frameworks/*.xcframework
	tar -xvf .cache/xcframeworks/libmpv-xcframeworks-macos-universal.tar.gz --strip-components 1 -C Frameworks
	touch Frameworks/*.xcframework

Frameworks/.symlinks: Frameworks/*.xcframework
	rm -rf Frameworks/.symlinks
	mkdir -p Frameworks/.symlinks/mpv
	sed -i '' 's/\r$$//g' create_framework_symlinks.sh # remove CRLF line terminator added by Flutter packaging (https://github.com/media-kit/media-kit/issues/338)
	sh create_framework_symlinks.sh Frameworks/Mpv.xcframework Frameworks/.symlinks/mpv

.PHONY: clean
clean:
	rm -rf .cache
	rm -f ./Headers/mpv/*.h
	rm -rf ./Frameworks/*.xcframework
