# Makefile for Presentations 2026 Repo
# -------------------------------------------
# Kellyn Gorman, Dec. 2025
# Usage:
#   make            # Build all PDFs (default)
#   make pdf        # Build PDFs from slides/*.md
#   make html       # Build HTML (reveal.js) versions
#   make clean      # Remove build artifacts
#   make help       # Show help information

SLIDES_DIR := slides
BUILD_DIR  := build

PANDOC := pandoc

# Pandoc options
PANDOC_PDF_FLAGS  := -t beamer
PANDOC_HTML_FLAGS := -t revealjs -s -V revealjs-url=https://unpkg.com/reveal.js/

SOURCES_MD  := $(wildcard $(SLIDES_DIR)/*.md)
PDF_SLIDES  := $(patsubst $(SLIDES_DIR)/%.md,$(BUILD_DIR)/%.pdf,$(SOURCES_MD))
HTML_SLIDES := $(patsubst $(SLIDES_DIR)/%.md,$(BUILD_DIR)/%.html,$(SOURCES_MD))

.PHONY: all
all: pdf

.PHONY: pdf
pdf: $(PDF_SLIDES)

.PHONY: html
html: $(HTML_SLIDES)

$(BUILD_DIR):
	mkdir -p $(BUILD_DIR)

$(BUILD_DIR)/%.pdf: $(SLIDES_DIR)/%.md | $(BUILD_DIR)
	$(PANDOC) $(PANDOC_PDF_FLAGS) $< -o $@

$(BUILD_DIR)/%.html: $(SLIDES_DIR)/%.md | $(BUILD_DIR)
	$(PANDOC) $(PANDOC_HTML_FLAGS) $< -o $@

.PHONY: clean
clean:
	rm -rf $(BUILD_DIR)

.PHONY: help
help:
	@echo "Presentation Slides Makefile"
	@echo ""
	@echo "Targets:"
	@echo "  make / make all   Build all PDF slides"
	@echo "  make pdf          Build PDF slides from Markdown"
	@echo "  make html         Build HTML reveal.js slides"
	@echo "  make clean        Remove build directory"
	@echo "  make help         Show this help message"
