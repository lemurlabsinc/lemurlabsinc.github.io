COLOR=black

.PHONY: help

help:
	@echo "Uses imagemagick, potrace and dwebp(optional) to convert raster"
	@echo "images to vector, and back to gif maintaining transparency"
	@echo ""
	@echo "uses pattern rules by suffix to support transformations:"
	@echo "  webp -> png -> ppm -> { eps , pgm, svg }; svg -> gif"

%.png: %.webp
	dwebp "$<" -o "$@"

# This step loses the alpha layer
# expand png files to ppm for use by potrace
%.ppm: %.png
	convert "$<" "$@"

%.ppm: %.xcf
	convert "$<" "$@"

# This step should use white as the alpha layer, but does not
# use potrace to convert to a vector in eps format
%.eps: %.ppm
	# potrace "$<" -o "$@"
	potrace "$<" -o "$@" --fillcolor "#aaff00"

# we can convert this one to maintain alpha.
# use potrace to convert to a vector in svg format
%.svg: %.ppm
	potrace -s "$<" -o "$@"

# use potrace to convert to a "vector" in pgm format
%.pgm: %.ppm
	potrace "$<" -o "$@" --pgm

%.gif: %.svg
	 convert -background transparent -fill "${COLOR}" -opaque black "$<" "$@"
