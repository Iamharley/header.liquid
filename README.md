# header.liquid
{%- liquid
  assign main_menu = linklists[section.settings.main_menu_link_list]
  assign menu_alignment = section.settings.main_menu_alignment
  assign logo_alignment = 'left'
  if menu_alignment == 'center-left' or menu_alignment == 'center-split' or menu_alignment == 'center'
    assign logo_alignment = 'center'
  endif
  if menu_alignment == 'right'
    assign logo_alignment = 'left-right'
  endif
  if main_menu == blank
    assign logo_alignment = 'center'
  endif
  assign mobile_menu_style = 'thumb'
  assign template_name = template | replace: '.', ' ' | truncatewords: 2, '' | handle
  assign overlay_header = false
  if template_name == 'index' and section.settings.sticky_index
    assign overlay_header = true
  endif
  if template_name contains 'collection' and collection.image and section.settings.sticky_collection
    assign overlay_header = true
  endif
-%}

{%- render 'slide-nav',
  section: section,
  main_menu: main_menu,
  mobile_menu_style: mobile_menu_style
-%}
{%- if settings.cart_type == 'sticky' -%}
  {%- render 'sticky-cart', mobile_menu_style: mobile_menu_style -%}
{%- endif -%}



<div data-section-id="{{ section.id }}" data-section-type="header-section">
  <div
    data-header-style="{{ section.settings.header_style }}"
    class="header-wrapper{% if overlay_header %} header-wrapper--overlay is-light{% endif %}">
    
    {%- assign mobile_logo_only = true -%}
    
    <header
      class="site-header{% if section.settings.logo_hide_mobile %}{% unless template_name == 'index' %} small--hide{% endunless %}{% endif %}"
      data-overlay="{{ overlay_header }}">
      
      <div class="page-width">
        <div
          class="header-layout header-layout--{{ menu_alignment }}{% if mobile_logo_only %} header-layout--mobile-logo-only{% endif %}"
          data-logo-align="{{ logo_alignment }}">
          
          {%- if logo_alignment == 'left' or logo_alignment == 'left-right' -%}
            <div class="header-item header-item--logo">
              {%- render 'header-logo-block', section: section -%}
              <div class="badge-promo">🔥 DROP</div>
            </div>
          {%- endif -%}
          
          {%- if logo_alignment == 'left' -%}
            <div role="navigation" aria-label="Primary"
              class="header-item header-item--navigation{% if menu_alignment == 'left-center' %} text-center{% endif %} small--hide">
              {%- render 'header-desktop-nav', main_menu: main_menu, hover_menu: section.settings.hover_menu -%}
            </div>
          {%- endif -%}
          
          {%- if logo_alignment == 'center' -%}
            <div class="header-item header-item--left header-item--navigation small--hide" role="navigation" aria-label="Primary">
              {%- if menu_alignment == 'center' or menu_alignment == 'center-split' -%}
                {%- if section.settings.header_search_enable -%}
                  <div class="site-nav">
                    <a href="{{ routes.search_url }}" class="site-nav__link site-nav__link--icon js-modal-open-search-modal js-no-transition">
                      <svg aria-hidden="true" focusable="false" role="presentation" class="icon icon-search" viewBox="0 0 64 64"><title>icon-search</title><path d="M47.16 28.58A18.58 18.58 0 1 1 28.58 10a18.58 18.58 0 0 1 18.58 18.58ZM54 54 41.94 42"/></svg>
                      <span class="icon__fallback-text">{{ 'general.search.title' | t }}</span>
                    </a>
                  </div>
                {%- endif -%}
              {%- endif -%}
              {%- if menu_alignment == 'center-left' -%}
                {%- render 'header-desktop-nav', main_menu: main_menu, hover_menu: section.settings.hover_menu -%}
              {%- endif -%}
            </div>
            
            {%- if menu_alignment == 'center-split' -%}
              {%- render 'header-split-nav', main_menu: main_menu, section: section, hover_menu: section.settings.hover_menu -%}
            {%- endif -%}
            
            {%- if menu_alignment != 'center-split' -%}
              <div class="header-item header-item--logo">
                {%- render 'header-logo-block', section: section -%}
                <div class="badge-promo">🔥 DROP</div>
              </div>
            {%- endif -%}
          {%- endif -%}
          
          {%- if logo_alignment == 'left-right' -%}
            <div
              role="navigation" aria-label="Primary"
              class="header-item header-item--navigation text-right small--hide">
              {%- render 'header-desktop-nav', main_menu: main_menu, hover_menu: section.settings.hover_menu -%}
            </div>
          {%- endif -%}
          
          <div class="header-item header-item--icons{% if mobile_logo_only %} small--hide{% endif %}">
            {%- render 'header-icons', section: section, menu_alignment: menu_alignment -%}
          </div>
        </div>
        
        {%- if menu_alignment == 'center' -%}
          <div role="navigation" aria-label="Primary" class="text-center">
            {%- render 'header-desktop-nav', main_menu: main_menu, hover_menu: section.settings.hover_menu -%}
          </div>
        {%- endif -%}
      </div>
    </header>
  </div>
  
  {%- if mobile_menu_style == 'thumb' -%}
    {%- if main_menu != blank -%}
      <div class="site-nav__thumb-menu site-nav__thumb-menu--inactive">
        <button
          type="button"
          class="btn site-nav__thumb-button js-toggle-slide-nav">
          <svg aria-hidden="true" focusable="false" role="presentation" class="icon icon-hamburger" viewBox="0 0 64 64"><title>icon-hamburger</title><path d="M7 15h51M7 32h43M7 49h51"/></svg>
          <svg aria-hidden="true" focusable="false" role="presentation" class="icon icon-close" viewBox="0 0 64 64"><title>icon-X</title><path d="m19 17.61 27.12 27.13m0-27.12L19 44.74"/></svg>
          <span class="icon-menu-label">{{ 'general.drawers.navigation' | t }}</span>
        </button>
        <a href="{{ routes.cart_url }}" class="site-nav__thumb-cart js-drawer-open-cart js-no-transition" aria-controls="CartDrawer" data-icon="{{ settings.cart_icon }}">
          <span class="cart-link">
            {%- if settings.cart_icon == 'cart' -%}
              <svg aria-hidden="true" focusable="false" role="presentation" class="icon icon-cart" viewBox="0 0 64 64"><title>icon-cart</title><path d="M14 17.44h46.79l-7.94 25.61H20.96l-9.65-35.1H3"/><circle cx="27" cy="53" r="2"/><circle cx="47" cy="53" r="2"/></svg>
            {%- else -%}
              <svg aria-hidden="true" focusable="false" role="presentation" class="icon icon-bag" viewBox="0 0 64 64"><g fill="none" stroke="#000" stroke-width="2"><path d="M25 26c0-15.79 3.57-20 8-20s8 4.21 8 20"/><path d="M14.74 18h36.51l3.59 36.73h-43.7z"/></g></svg>
            {%- endif -%}
            <span class="icon__fallback-text">{{ 'layout.cart.title' | t }}</span>
            <span class="cart-link__bubble{% if cart.item_count > 0 %} cart-link__bubble--visible{% endif %}">
              <span class="cart-link__bubble-num" aria-hidden="true">
                {{ cart.item_count }}
              </span>
            </span>
          </span>
        </a>
      </div>
    {%- endif -%}
  {%- endif -%}
</div>

{%- if section.settings.header_search_enable -%}
  {%- render 'search-modal' -%}
{%- endif -%}

{% schema %}
{
  "name": "Header Harley Relief",
  "settings": [
    {
      "type": "select",
      "id": "header_style",
      "label": "Header Style",
      "default": "relief",
      "options": [
        {
          "value": "relief",
          "label": "Relief Authentique"
        },
        {
          "value": "classic",
          "label": "Classic"
        }
      ]
    },
    {
      "type": "select",
      "id": "main_menu_alignment",
      "label": "Menu Alignment",
      "default": "right",
      "options": [
        {
          "value": "right",
          "label": "Right"
        },
        {
          "value": "left-center",
          "label": "Left Center"
        },
        {
          "value": "center-left",
          "label": "Center Left"
        },
        {
          "value": "center-split",
          "label": "Center Split"
        },
        {
          "value": "center",
          "label": "Center"
        }
      ]
    },
    {
      "type": "checkbox",
      "id": "sticky_index",
      "label": "Sticky on Homepage"
    },
    {
      "type": "checkbox",
      "id": "sticky_collection",
      "label": "Sticky on Collection Pages"
    },
    {
      "type": "checkbox",
      "id": "header_search_enable",
      "label": "Enable Search"
    },
    {
      "type": "image_picker",
      "id": "logo",
      "label": "Logo"
    },
    {
      "type": "image_picker",
      "id": "logo-inverted",
      "label": "Logo Inverted"
    },
    {
      "type": "range",
      "id": "desktop_logo_width",
      "label": "Desktop Logo Width",
      "default": 200,
      "min": 80,
      "max": 400,
      "step": 10,
      "unit": "px"
    },
    {
      "type": "range",
      "id": "mobile_logo_width",
      "label": "Mobile Logo Width",
      "default": 140,
      "min": 50,
      "max": 200,
      "step": 10,
      "unit": "px"
    },
    {
      "type": "checkbox",
      "id": "logo_hide_mobile",
      "label": "Hide Logo on Mobile",
      "default": true
    },
    {
      "type": "link_list",
      "id": "main_menu_link_list",
      "label": "Menu",
      "default": "main-menu"
    },
    {
      "type": "checkbox",
      "id": "hover_menu",
      "label": "Enable Hover Menus",
      "default": true
    }
  ]
}
{% endschema %}
