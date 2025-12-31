# Enfold Shortcode Reference

This guide covers the Enfold theme shortcodes used on the Your Site website.

## Basic Structure

Enfold pages are built with nested shortcode elements:

```
[av_section]                    ← Full-width section container
  [av_one_full first]           ← Column (first needs 'first' attr)
    [av_heading]...[/av_heading]  ← Content elements
    [av_textblock]...[/av_textblock]
  [/av_one_full]
[/av_section]
```

## Section Container

```
[av_section min_height='' min_height_pc='25' min_height_px='500px' padding='default' shadow='no-border-styling' bottom_border='no-border-styling' id='' color='main_color' custom_bg='' background_gradient_color1='' background_gradient_color2='' background_gradient_direction='vertical' av-desktop-hide='' av-medium-hide='' av-small-hide='' av-mini-hide='' av_uid='av-xxxxx']

... content ...

[/av_section]
```

**Key attributes:**
- `padding` - `default`, `small`, `large`, `huge`, `no-padding`
- `color` - `main_color`, `alternate_color`, `header_color`, `footer_color`
- `custom_bg` - Hex color for custom background
- `id` - HTML anchor ID for linking

## Column Layouts

### Full Width
```
[av_one_full first]...[/av_one_full]
```

### Two Columns
```
[av_one_half first]...[/av_one_half]
[av_one_half]...[/av_one_half]
```

### Three Columns
```
[av_one_third first]...[/av_one_third]
[av_one_third]...[/av_one_third]
[av_one_third]...[/av_one_third]
```

### Four Columns
```
[av_one_fourth first]...[/av_one_fourth]
[av_one_fourth]...[/av_one_fourth]
[av_one_fourth]...[/av_one_fourth]
[av_one_fourth]...[/av_one_fourth]
```

### Mixed Layouts
```
[av_two_third first]...[/av_two_third]
[av_one_third]...[/av_one_third]
```

**Important:** The first column must have `first` attribute!

## Content Elements

### Heading

```
[av_heading heading='Your Heading Text' tag='h1' style='blockquote modern-quote' size='' subheading_active='subheading_below' subheading_size='15' padding='10' color='' custom_font='' av_uid='']Optional subheading text[/av_heading]
```

**Key attributes:**
- `tag` - `h1`, `h2`, `h3`, `h4`, `h5`, `h6`
- `style` - `blockquote modern-quote`, `blockquote classic-quote`
- `subheading_active` - `subheading_below`, `subheading_above`, or empty

### Text Block

```
[av_textblock size='' font_color='' color='' av_uid='']
<p>Your HTML content here.</p>
<p>Supports <strong>bold</strong>, <em>italic</em>, <a href="url">links</a>.</p>
[/av_textblock]
```

### Button

```
[av_button label='Button Text' link='manually,https://example.com' link_target='_blank' size='large' position='center' icon_select='no' icon='ue800' font='entypo-fontello' color='theme-color' custom_bg='#444444' custom_font='#ffffff' av_uid='']
```

**Key attributes:**
- `link` - `manually,URL` for external links
- `link_target` - `_blank` for new tab, empty for same tab
- `size` - `small`, `medium`, `large`, `x-large`
- `position` - `left`, `center`, `right`
- `color` - `theme-color`, `custom`

### Image

```
[av_image src='https://example.com/wp-content/uploads/image.jpg' attachment='' attachment_size='' align='center' styling='' hover='' link='' target='' caption='' font_size='' appearance='' overlay_opacity='0.4' overlay_color='#000000' overlay_text_color='#ffffff' av_uid=''][/av_image]
```

### Icon Box

```
[av_icon_box icon='ue862' font='entypo-fontello' title='Feature Title' position='top' icon_style='' boxed='' font_color='' custom_title='' custom_content='' color='' custom_bg='' custom_font='' custom_border='' av_uid='']
Description text here.
[/av_icon_box]
```

### Horizontal Rule

```
[av_hr class='default' height='50' shadow='no-shadow' position='center' custom_border='av-border-thin' custom_width='50px' custom_border_color='' custom_margin_top='30px' custom_margin_bottom='30px' icon_select='yes' custom_icon_color='' icon='ue808' av_uid='']
```

## Special Elements

### Google Map

```
[av_google_map height='400px' zoom='16' saturation='' hue='' zoom_control='aviaTBzoom_control' maptype_control='' av_uid='']
[av_gmap_location address='123 Main St, City' city='City' country='Country' long='-0.123456' lat='51.123456' marker='' imagesize='40' av_uid=''][/av_gmap_location]
[/av_google_map]
```

### Contact Form

```
[av_contact email='info@example.com' title='Contact Us' button='Send' on_send='' sent='Your message has been sent!' link='manually,http://example.com/thank-you' captcha='' captcha_theme='light' captcha_size='normal' captcha_score='0.5' multi_select='' av_uid='']
[av_contact_field label='Name' type='text' options='' check='is_empty' width='' av_uid=''][/av_contact_field]
[av_contact_field label='Email' type='text' options='' check='is_email' width='' av_uid=''][/av_contact_field]
[av_contact_field label='Message' type='textarea' options='' check='is_empty' width='' av_uid=''][/av_contact_field]
[/av_contact]
```

### Tab Section

```
[av_tab_section tab_pos='av-tab-above-content' content_height='av-tab-content-auto' av_uid='']
[av_tab_sub_section tab_title='Tab 1' av_uid='']
... content ...
[/av_tab_sub_section]
[av_tab_sub_section tab_title='Tab 2' av_uid='']
... content ...
[/av_tab_sub_section]
[/av_tab_section]
```

## Common Patterns

### Hero Section with Background

```
[av_section min_height='50' min_height_pc='25' min_height_px='500px' padding='large' shadow='no-border-styling' bottom_border='no-border-styling' id='' color='main_color' custom_bg='#1a1a2e' background_gradient_color1='' background_gradient_color2='' src='https://example.com/wp-content/uploads/hero-bg.jpg' attachment='12345' attachment_size='full' attach='parallax' position='center center' repeat='stretch' video='' av_uid='']

[av_one_full first]

[av_heading heading='Welcome' tag='h1' style='blockquote modern-quote' color='custom-color-heading' custom_font='#ffffff' av_uid=''][/av_heading]

[/av_one_full]

[/av_section]
```

### Card Grid

```
[av_section]
[av_one_third first]
[av_icon_box icon='ue862' title='Feature 1' position='top']Description[/av_icon_box]
[/av_one_third]
[av_one_third]
[av_icon_box icon='ue864' title='Feature 2' position='top']Description[/av_icon_box]
[/av_one_third]
[av_one_third]
[av_icon_box icon='ue866' title='Feature 3' position='top']Description[/av_icon_box]
[/av_one_third]
[/av_section]
```

## Tips

1. **Always close shortcodes** - Every `[av_xxx]` needs `[/av_xxx]`
2. **Keep av_uid unique** - Or leave empty to auto-generate
3. **Use relative URLs** - `/page-slug/` instead of full URLs where possible
4. **Test on staging** - Shortcode errors can break the entire page
5. **Preserve whitespace** - Some shortcodes are sensitive to line breaks
