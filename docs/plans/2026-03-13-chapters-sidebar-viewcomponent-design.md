# Chapters Sidebar ViewComponent Design

## Overview
Replace the current chapters sidebar implementation in `app/views/dashboard/show.html.haml` with a ViewComponent that includes fragment caching.

## Current Implementation
- Located in `app/views/dashboard/show.html.haml` lines 79-85
- Uses `@chapters` from `Chapter.active.all.order(:created_at)`
- Simple list of chapter names linking to chapter pages

## Proposed Solution
Create `ChaptersSidebarComponent` that:
1. Renders the same chapter list markup
2. Includes fragment caching with cache key based on `Chapter.active.maximum(:updated_at)`
3. Can be rendered with `render ChaptersSidebarComponent.new(chapters: @chapters)`

## Component Structure
- `app/components/chapters_sidebar_component.rb` - Ruby component class
- `app/components/chapters_sidebar_component.html.haml` - HAML template

## Caching Strategy
- Cache key: `chapters-sidebar/{timestamp}`
- Timestamp from `Chapter.active.maximum(:updated_at)` to invalidate when chapters change
- Works with any Rails cache store

## Benefits
- Reusable component for potential use elsewhere
- Automatic caching for better performance
- Cleaner view templates
- Easy to extend with additional features (icons, descriptions, etc.)
