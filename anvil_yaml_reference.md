# Anvil YAML Component Configuration Guide

This document explains the YAML format used by Anvil's IDE to define UI components. Use this reference to generate and maintain form YAML files programmatically.

## Form File Structure
Each Anvil form is represented in the filesystem as:
```
FormName/
  __init__.py         # Python code for the form
  form_template.yaml  # YAML configuration 
```

The YAML file (form_template.yaml) contains:
- Component hierarchy and properties
- Layout information
- Data/event bindings
- Form metadata

This document focuses on the structure and syntax of these form_template.yaml files.

## Form YAML Structure
```yaml
container:  # Root container configuration
  type: ColumnPanel
  event_bindings: 
    refreshing_data_bindings: form_refreshing_data_bindings
components:
  - name: plaintext_card  # Component name (for IDE display)
    type: ColumnPanel
    properties: 
      role: outlined-card
    layout_properties:  # Visual layout in designer
      grid_position: 'OLMVBV,NYGJZB' 
      full_width_row: true
    components:  # Nested components
      - type: RichText
        data_bindings:  # Data binding configuration
          - property: text
            code: self.some_property
            writeback: true
        event_bindings:  # Event handler mapping
          change: handler_function
is_package: true  # Mark as reusable component
```

## Key Components

### 1. Component Definition
```yaml
- name: component_name  # Designer-friendly name
  type: ComponentType   # e.g. Button, custom.CompositeComponent
  id: unique_id         # Python identifier
  properties:           # Component settings
    text: "Submit"
    role: outlined-button
  layout_properties:    # Designer layout
    grid_position: 'X,Y' 
    full_width_row: true
  data_bindings:        # Data bindings
    - property: enabled
      code: self.some_condition 
      writeback: false
  event_bindings:       # Event handlers
    click: click_handler
  components:           # Nested components
    - type: ...
```

### 2. Common Component Types and Properties

#### DataRowPanel
```yaml
type: DataRowPanel
properties:
  background: "theme:Gray 200"
  spacing_above: none
  auto_display_data: true
```

#### RepeatingPanel
```yaml 
type: RepeatingPanel
properties:
  item_template: content.package.ItemTemplate
  items: []
  spacing_above: none
```

#### FlowPanel
```yaml
type: FlowPanel
properties:
  align: right
  spacing: medium
  vertical_align: middle
```

#### Button
```yaml
type: Button
properties:
  text: "Submit"
  role: primary-color
  icon: "fa:icon-name"
  enabled: false
  visible: true
```

#### TextBox 
```yaml
type: TextBox
properties:
  placeholder: "Enter text"
  type: "text"  # text|number|email|tel|url
  hide_text: false
  auto_expand: true  # For multi-line text boxes
  height: 300  # Fixed height in pixels
  enabled: false  # Disable editing
```

#### TextArea
```yaml
type: TextArea 
properties:
  placeholder: "Long-form text"
  auto_expand: false  # Default, set true for auto-growing
  height: 150  # Initial height
```

#### DataGrid
```yaml
type: DataGrid
properties:
  auto_header: true
  rows_per_page: 10
  show_page_controls: false
  columns:
    - name: "Column 1"
      id: col1
      data_key: "field_name" 
      width: "200px"
      formatter: "progress"  # Special formatters
    - name: "Actions"
      components:
        - type: Button
          text: "Edit"
          role: primary-color
```

### 3. Event Handling
```yaml
event_bindings:
  click: button_click_handler
  change: textbox_changed
  show: form_shown
  hover: element_hover  # Mouse enter/leave events
  pressed_enter: text_entered
  # File-specific events:
  file: file_loader_change  
  # Component-specific events:
  cell_click: tabulator_cell_click
```

### 4. Containers and Layout
```yaml
- type: ColumnPanel
  components:
    - type: Label
      text: "Section Header"
      layout_properties:
        full_width_row: true
        row_background: "#f0f0f0"
    - type: TextBox
      id: input_field
  properties:
    col_spacing: medium
    wrap_on: mobile  # Break columns on mobile
    background: theme:secondary

# GridPanel layout example
- type: GridPanel
  components:
    - type: Button
      text: "Submit"
      layout_properties:
        row: 0
        col_xs: 0
        width_xs: 6
```

### 5. Form Configuration
```yaml
title: "User Login"  # Shown in browser tab
is_package: false  # Marks as reusable component
container: ColumnPanel  # Root container type
properties:
  spacing_above: none
  spacing_below: small
  title_align: center
components:
  - type: EmailTextBox
    id: email_input
    properties:
      placeholder: "user@example.com"
```
1. **ID Naming**: Use snake_case and prefix with component type (`button_submit`, `textbox_email`)
2. **Hierarchy**: Maintain visual hierarchy in YAML structure
3. **Custom Props**: Store non-UI data in `tag` property
```yaml
properties:
  tag:
    custom_field: "value"
```

## Updating Existing YAMLs
When modifying forms:
1. Append new components to the `components` array
2. Preserve existing component order
3. Update properties with minimal diffs

**Example Update:**
```yaml
# Before
components:
  - type: Label
    text: "Old Header"

# After  
components:
  - type: Label
    text: "New Header"
  - type: Button
    text: "New Button"
```

### 6. Custom Components
```yaml
- type: form:CLIENT_ID:Switch  # From custom components package
  properties:
    text_post: 'Is Admin?'
- type: form:DEPENDENCY_ID:Tabulator  # Custom tabulator component
  event_bindings:
    row_click: tabulator_row_clicked
- type: form:components.Pivot  # From shared components package
  properties:
    rows: [provenance_id]
    cols: [source_id] 
  id: custom_component
  properties: {}
  event_bindings:
    value_changed: custom_component_changed
  layout_properties:
    grid_position: 'A1B2C3,D4E5F6'
# External component reference format:
# form:DEPENDENCY_ID:MODULE_PATH.COMPONENT_NAME
```

### 7. Form Inheritance
```yaml
inherits: BaseForm  # Parent form template
properties:
  # Override parent properties
  background: "#f5f5f5"  
components:
  # Add child-specific components
  - type: Button
    text: "Submit"
  # Preserve parent components automatically
```
Ensure YAML files:
- Have proper indentation (2 spaces)
- Use valid component types from Anvil API
- Include required properties:
  - `type` for all components
  - `id` for non-container components
  - `handler` functions must exist in form code

Refer to Anvil's [Component API Docs](https://anvil.works/docs/api/anvil) for full property lists.
