# Enfold WordPress Theme - Page Builder Knowledge Base

## Overview

Enfold uses shortcodes to build page layouts. This document provides the complete reference for generating valid Enfold pages.

## Core Concepts

### Hierarchy
```
av_section (Color Section - fullwidth container)
  └── av_one_half / av_one_third / etc. (Columns)
        └── Content elements (buttons, text, images, etc.)
```

### Shortcode Syntax
```
[shortcode_name param1='value1' param2='value2']content[/shortcode_name]
```
Or self-closing:
```
[shortcode_name param1='value1' /]
```

---

## LAYOUT ELEMENTS

### Color Section (`av_section`)
Main fullwidth container for page sections.

```shortcode
[av_section min_height='' min_height_pc='25' min_height_px='500px' shadow='no-border-styling' bottom_border='no-border-styling' padding='default' margin='' color='main_color' background='bg_color' custom_bg='' src='' attach='scroll' video='' overlay_enable='' overlay_opacity='0.5' overlay_color='']

[/av_section]
```

**Key Parameters:**
- `min_height`: '', '100', '75', '50', '25', 'custom', 'percent'
- `padding`: 'no-padding', 'small', 'default', 'large', 'huge'
- `color`: 'main_color', 'alternate_color', 'header_color', 'footer_color', 'socket_color'
- `background`: 'bg_color', 'bg_gradient', 'bg_alternate'
- `src`: URL or attachment ID for background image
- `video`: YouTube/Vimeo URL for background video

---

### Columns
Columns must be inside a section. First column needs `first` attribute.

**Available Column Widths:**
```shortcode
[av_one_full first]...[/av_one_full]
[av_one_half first]...[/av_one_half]
[av_one_half]...[/av_one_half]
[av_one_third first]...[/av_one_third]
[av_one_third]...[/av_one_third]
[av_two_third first]...[/av_two_third]
[av_one_fourth first]...[/av_one_fourth]
[av_three_fourth first]...[/av_three_fourth]
[av_one_fifth first]...[/av_one_fifth]
```

**Column Parameters:**
```shortcode
[av_one_half first min_height='' vertical_alignment='av-align-top' space='' row_boxshadow_width='10' row_boxshadow_color='' margin='0px' margin_sync='true' padding='0px' padding_sync='true' highlight_size='1.1' border='' border_color='' radius='0px' radius_sync='true' column_boxshadow_width='10' column_boxshadow_color='' background='bg_color' background_color='' src='' attachment='' background_position='top left' background_repeat='no-repeat' animation='']
```

---

### Grid Row (`av_layout_row`)
Advanced grid layout with customizable cells.

```shortcode
[av_layout_row border='' min_height='0' color='main_color' mobile='av-flex-cells' mobile_breaking='']
    [av_cell_one_half vertical_align='top' padding='30px' background='bg_color']
    [/av_cell_one_half]
    [av_cell_one_half vertical_align='top' padding='30px' background='bg_alternate']
    [/av_cell_one_half]
[/av_layout_row]
```

---

### Tab Section (`av_tab_section`)
Horizontal tabbed content sections.

```shortcode
[av_tab_section initial='1' tab_pos='av-tab-above-content' content_height='' tab_padding='default' padding='default' bg_color='' color='']
    [av_tab_sub_section tab_title='Tab 1' icon_select='no' vertical_align='middle' color='']
        Content here
    [/av_tab_sub_section]
    [av_tab_sub_section tab_title='Tab 2' icon_select='no']
        More content
    [/av_tab_sub_section]
[/av_tab_section]
```

---

## CONTENT ELEMENTS

### Text Block (`av_textblock`)
Rich text content.

```shortcode
[av_textblock textblock_styling='' textblock_styling_align='' textblock_styling_size='' textblock_styling_gap='' size='' av-desktop-font-size='' av-medium-font-size='' av-small-font-size='' av-mini-font-size='' font_color='' color='' alb_description='' id='' custom_class='' av_uid='']
Your text content here. Can include <strong>HTML</strong> and formatting.
[/av_textblock]
```

---

### Special Heading (`av_heading`)
Styled headings with optional subheading.

```shortcode
[av_heading heading='Your Heading' tag='h2' style='blockquote modern-quote modern-centered' subheading_active='subheading_below' show_icon='' icon='ue800' font='' size='40' av-desktop-font-size='' av-medium-font-size='' av-small-font-size='' av-mini-font-size='' subheading_size='' av-desktop-font-size-1='' padding='10' heading_padding='' color='' custom_font='' icon_padding='10' subheading_color='' seperator_color='' margin='' margin_sync='true' av-desktop-margin='' av-desktop-margin_sync='true' custom_class='' id='' av_uid='']
Optional subheading text
[/av_heading]
```

**Style Options:**
- `blockquote modern-quote`: Modern quote style
- `blockquote modern-quote modern-centered`: Centered modern quote
- `blockquote classic-quote`: Classic quote style

---

### Button (`av_button`)
Single button element.

```shortcode
[av_button label='Click Me' icon_select='yes' icon='ue800' font='entypo-fontello' icon_hover='aviaTBicon_hover' link='manually,https://example.com' link_target='_blank' size='large' position='center' label_display='' title_attr='' size-text='' av-desktop-font-size-text='' color_options='' color='theme-color' custom_bg='#444444' custom_font='#ffffff' btn_color_bg='theme-color' btn_custom_grad_direction='vertical' btn_custom_grad_1='#000000' btn_custom_grad_2='#ffffff' btn_custom_grad_3='' btn_custom_grad_opacity='0.7' btn_custom_bg='#444444' btn_color_bg_hover='theme-color-highlight' btn_custom_bg_hover='#444444' btn_color_font='theme-color' btn_custom_font='#ffffff' btn_color_font_hover='white' btn_custom_font_hover='#ffffff' border='' border_width='' border_width_sync='true' border_color='' border_radius='' border_radius_sync='true' box_shadow='' box_shadow_style='0px,0px,0px,0px' box_shadow_color='' animation='' id='' custom_class='' av_uid='']
```

**Size Options:** 'small', 'medium', 'large', 'x-large'
**Position:** 'left', 'center', 'right'
**Color:** 'theme-color', 'theme-color-highlight', 'theme-color-subtle', 'light', 'dark', 'custom'

---

### Button Row (`av_buttonrow`)
Multiple buttons in a row.

```shortcode
[av_buttonrow alignment='center' button_spacing='5' button_spacing_unit='px']
    [av_buttonrow_item label='Button 1' icon_select='no' link='manually,#' link_target='' size='small' color='theme-color' /]
    [av_buttonrow_item label='Button 2' icon_select='no' link='manually,#' link_target='' size='small' color='theme-color-highlight' /]
[/av_buttonrow]
```

---

### Icon Box (`av_icon_box`)
Icon with title and description.

```shortcode
[av_icon_box icon='ue800' font='entypo-fontello' title='Box Title' position='left' icon_style='' boxed='' font_color='' custom_title='' custom_content='' color='' custom_bg='' custom_font='' custom_border='' custom_title_size='' av-desktop-font-size-title='' custom_content_size='' av-desktop-font-size='' heading_tag='' heading_class='' link='manually,http://' link_target='' linktarget='' linkelement='both' id='' custom_class='' av_uid='']
Description text goes here.
[/av_icon_box]
```

**Position:** 'left', 'right', 'top', 'left_content', 'right_content'

---

### Icon List (`av_iconlist`)
Bulleted list with custom icons.

```shortcode
[av_iconlist position='left' iconlist_styling='' custom_title_size='' custom_content_size='' font_color='' custom_title='' custom_content='' color='' custom_bg='' custom_font='' custom_border='' av-desktop-font-size-title='' av-medium-font-size-title='' av-small-font-size-title='' av-mini-font-size-title='' av-desktop-font-size='' heading_tag='' heading_class='' id='' custom_class='' av_uid='']
    [av_iconlist_item title='Item 1' icon='ue812' font='entypo-fontello' heading_tag='' heading_class='' link='' linktarget='' linkelement='']
    Description for item 1
    [/av_iconlist_item]
    [av_iconlist_item title='Item 2' icon='ue812' font='entypo-fontello']
    Description for item 2
    [/av_iconlist_item]
[/av_iconlist]
```

---

### Testimonials (`av_testimonials`)
Customer testimonials slider.

```shortcode
[av_testimonials style='slider' columns='2' grid_style='' interval='5' font_color='' custom_title='' custom_content='' alb_description='' id='' custom_class='' av_uid='']
    [av_testimonial_single src='' name='John Doe' subtitle='CEO, Company' link='http://' linktext='']
    "This is an amazing testimonial quote."
    [/av_testimonial_single]
    [av_testimonial_single src='' name='Jane Smith' subtitle='Designer']
    "Another great testimonial here."
    [/av_testimonial_single]
[/av_testimonials]
```

---

### Team Member (`av_team_member`)
Team member profile card.

```shortcode
[av_team_member name='John Doe' job='CEO & Founder' src='image-url.jpg' attachment='' description='' font_color='' custom_title='' custom_content='' id='' custom_class='' av_uid='']
Bio text goes here.
[/av_team_member]
```

---

### Progress Bars (`av_progress`)
Animated percentage bars.

```shortcode
[av_progress bar_styling='av-striped-bar' bar_animation='av-animated-bar' bar_styling_secondary='' show_percentage='' bar_height='10' id='' custom_class='' av_uid='']
    [av_progress_bar title='HTML' progress='90' progress_max='100' color='theme-color' custom_bg='#444444' /]
    [av_progress_bar title='CSS' progress='85' color='theme-color-highlight' /]
    [av_progress_bar title='JavaScript' progress='75' color='theme-color-subtle' /]
[/av_progress]
```

---

### Accordion / Toggles (`av_toggle_container`)
Collapsible content sections.

```shortcode
[av_toggle_container initial='0' mode='accordion' sort='' styling='' colors='' font_color='' background_color='' border_color='' hover_colors='' hover_background_color='' hover_font_color='' id='' custom_class='' av_uid='']
    [av_toggle title='First Question' tags='' custom_id='' av_uid='']
    Answer to first question.
    [/av_toggle]
    [av_toggle title='Second Question' tags='']
    Answer to second question.
    [/av_toggle]
[/av_toggle_container]
```

---

### Tabs (`av_tab_container`)
Tabbed content.

```shortcode
[av_tab_container position='top_tab' boxed='border_tabs' initial='1' id='' custom_class='' av_uid='']
    [av_tab title='Tab 1' icon_select='no' icon='ue800' font='entypo-fontello' av_uid='']
    Content for tab 1
    [/av_tab]
    [av_tab title='Tab 2' icon_select='no' av_uid='']
    Content for tab 2
    [/av_tab]
[/av_tab_container]
```

---

### Notification / Alert (`av_notification`)
Alert boxes.

```shortcode
[av_notification title='Note' icon_select='yes' icon='ue81e' font='entypo-fontello' color='green' border='' custom_bg='#444444' custom_font='#ffffff' size='large' close_btn='' cookie_lifetime='60' id='' custom_class='' av_uid='']
Your notification message here.
[/av_notification]
```

**Color Options:** 'green', 'blue', 'red', 'orange', 'silver', 'custom'

---

### Separator / Whitespace (`av_hr`)
Horizontal rule or spacing.

```shortcode
[av_hr class='default' icon_select='no' icon='ue808' font='entypo-fontello' position='center' shadow='no-shadow' height='50' custom_border='av-border-thin' custom_width='50px' custom_margin_top='30px' custom_margin_bottom='30px' custom_border_color='' custom_icon_color='' id='' custom_class='' av_uid='']
```

**Class Options:** 'default', 'short', 'full', 'invisible', 'custom'

---

## MEDIA ELEMENTS

### Image (`av_image`)
Single image with optional link.

```shortcode
[av_image src='image-url.jpg' attachment='123' attachment_size='full' copyright='' caption='' image_size='' styling='' box_shadow='none' box_shadow_width='10' box_shadow_color='' align='center' alb_description='' id='' custom_class='' av_uid='' link='manually,http://example.com' target='_blank' animation='no-animation' hover='av-hover-grow']
[/av_image]
```

**Styling:** '', 'no-styling', 'circle', 'rounded'
**Animation:** 'no-animation', 'fade-in', 'slide-up', 'slide-down', 'slide-left', 'slide-right', 'pop-up'

---

### Gallery (`av_gallery`)
Image gallery with lightbox.

```shortcode
[av_gallery ids='1,2,3,4,5' style='thumbnails' preview_size='portfolio' crop_big_preview_thumbnail='avia-gallery-big-crop-thumb' thumb_size='portfolio' columns='5' imagelink='lightbox' link_dest='' lightbox_text='caption' lazyload='avia_lazyload' id='' custom_class='' av_uid='']
```

**Style:** 'thumbnails', 'big_thumb'

---

### Video (`av_video`)
Embedded video.

```shortcode
[av_video src='https://www.youtube.com/watch?v=VIDEO_ID' mobile_image='' attachment='' attachment_size='' format='16-9' width='16' height='9' conditional_play='' id='' custom_class='' av_uid='']
```

---

### Easy Slider (`av_slideshow`)
Image slideshow.

```shortcode
[av_slideshow size='featured' animation='slide' autoplay='false' interval='5' control_layout='av-control-default' perma_caption='' id='' custom_class='' av_uid='']
    [av_slide id='123' slide_type='' video='http://' mobile_image='' video_ratio='16:9' video_controls='' video_mute='' video_loop='' video_autoplay='' title='Slide Title' heading_tag='' heading_class='' link_apply='' link='' link_target='']
    Slide caption text
    [/av_slide]
[/av_slideshow]
```

---

### Content Slider (`av_content_slider`)
Content blocks as slides.

```shortcode
[av_content_slider heading='' navigation='arrows' columns='1' font_color='' color='' heading_tag='' heading_class='' autoplay='false' interval='5' id='' custom_class='' av_uid='']
    [av_content_slide title='Slide 1' heading_tag='' heading_class='' link='' linktarget='']
    Content for slide 1
    [/av_content_slide]
    [av_content_slide title='Slide 2']
    Content for slide 2
    [/av_content_slide]
[/av_content_slider]
```

---

### Google Map (`av_google_map`)
Embedded map.

```shortcode
[av_google_map height='400px' zoom='16' saturation='' hue='' zoom_control='aviaTBzoom_control' maptype_control='' id='' custom_class='' av_uid='']
    [av_gmap_location address='123 Main St, City, Country' city='' country='' long='-122.4194' lat='37.7749' marker='' imagesize='40' tooltip_display='av-marker-tooltip-on-hover']
    Optional tooltip content
    [/av_gmap_location]
[/av_google_map]
```

---

## DYNAMIC CONTENT

### Blog Posts (`av_blog`)
Display blog posts.

```shortcode
[av_blog blog_type='posts' categories='' link='category' blog_style='single-big' content_length='content' date_filter='' date_filter_start='' date_filter_end='' date_filter_format='yy/mm/dd' period_filter_unit_1='1' period_filter_unit_2='year' items='3' offset='0' paginate='yes' conditional='' id='' custom_class='' av_uid='']
```

---

### Portfolio Grid (`av_portfolio`)
Display portfolio items.

```shortcode
[av_portfolio categories='' columns='4' one_column_template='special' items='16' contents='excerpt' preview_mode='auto' image_size='portfolio' linking='' sort='yes' paginate='yes' query_orderby='date' query_order='DESC' id='' custom_class='' av_uid='']
```

---

### Masonry (`av_masonry_entries`)
Masonry layout for posts/portfolio.

```shortcode
[av_masonry_entries link='category' wc_prod_visible='' wc_prod_hidden='hide' wc_prod_featured='' sort='yes' query_orderby='date' query_order='DESC' size='fixed' gap='large' overlay_fx='active' id='' caption_elements='title excerpt' caption_styling='' caption_display='always' color='' custom_bg='' items='12' columns='flexible' paginate='pagination' lazy_loading='disabled' custom_class='' av_uid='']
```

---

## EXAMPLE PAGE TEMPLATES

### Simple Landing Page
```shortcode
[av_section min_height='100' padding='large' color='main_color' custom_bg='#ffffff']
    [av_one_full first]
        [av_heading heading='Welcome to Our Site' tag='h1' style='blockquote modern-quote modern-centered']
        Your tagline here
        [/av_heading]
        [av_textblock]
        <p style="text-align: center;">Introduction text about your company or service.</p>
        [/av_textblock]
        [av_buttonrow alignment='center']
            [av_buttonrow_item label='Learn More' link='manually,#about' size='large' color='theme-color' /]
            [av_buttonrow_item label='Contact Us' link='manually,#contact' size='large' color='theme-color-highlight' /]
        [/av_buttonrow]
    [/av_one_full]
[/av_section]

[av_section padding='large' color='alternate_color' id='about']
    [av_one_third first]
        [av_icon_box icon='ue862' font='entypo-fontello' title='Feature One' position='top']
        Description of feature one.
        [/av_icon_box]
    [/av_one_third]
    [av_one_third]
        [av_icon_box icon='ue84d' font='entypo-fontello' title='Feature Two' position='top']
        Description of feature two.
        [/av_icon_box]
    [/av_one_third]
    [av_one_third]
        [av_icon_box icon='ue8ab' font='entypo-fontello' title='Feature Three' position='top']
        Description of feature three.
        [/av_icon_box]
    [/av_one_third]
[/av_section]
```

### Services Page
```shortcode
[av_section min_height='' padding='large' color='main_color' src='background-image.jpg' attach='parallax']
    [av_one_full first]
        [av_heading heading='Our Services' tag='h1' style='blockquote modern-quote modern-centered' color='custom-color-heading' custom_font='#ffffff'][/av_heading]
    [/av_one_full]
[/av_section]

[av_section padding='large']
    [av_one_half first]
        [av_image src='service1.jpg' align='center' styling='rounded'][/av_image]
    [/av_one_half]
    [av_one_half]
        [av_heading heading='Service One' tag='h2'][/av_heading]
        [av_textblock]
        Detailed description of service one. Explain benefits and features.
        [/av_textblock]
        [av_button label='Learn More' link='manually,/service-one' size='medium' color='theme-color' /]
    [/av_one_half]
[/av_section]
```

---

## COMMON ICON CODES (entypo-fontello)

- `ue800` - Home
- `ue803` - Search
- `ue806` - Mail
- `ue808` - Star
- `ue80a` - Heart
- `ue812` - Check
- `ue81e` - Info
- `ue821` - Warning
- `ue836` - Phone
- `ue840` - Location
- `ue854` - Clock
- `ue862` - Light bulb
- `ue84d` - Settings/Gear
- `ue8ab` - Chat

---

## NOTES

1. Always close shortcodes properly: `[shortcode]...[/shortcode]`
2. First column in a row needs `first` attribute
3. Fullwidth elements (section, slideshow) don't need columns
4. Use `av_uid=''` for unique IDs (can be left empty)
5. Colors reference theme color scheme settings
6. Image attachments can use URL or WordPress attachment ID
