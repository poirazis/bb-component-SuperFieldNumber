# Super Field - Number

A specialized number input field component for Budibase applications with numeric validation, formatting, and mathematical operations support.

## 🚀 Features

### Numeric Input

- **Number Field Binding**: Dedicated number field type with automatic type handling
- **Default Values**: Pre-populated numeric values
- **Placeholder Support**: Guidance text for numeric inputs
- **Auto-formatting**: Automatic number formatting and display

### Validation & Constraints

- **Numeric Validation**: Built-in number validation with min/max ranges
- **Decimal Precision**: Configurable decimal places and precision
- **Range Checking**: Minimum and maximum value constraints
- **Custom Validation Rules**: Advanced validation with error messages

### Formatting & Display

- **Value Templates**: Custom formatting templates for display
- **Locale Support**: Number formatting based on user locale
- **Currency Display**: Optional currency symbol and formatting
- **Unit Labels**: Support for measurement units and labels

### User Experience

- **Event Handling**: On change events with numeric context
- **Auto-focus**: Automatic focus for data entry workflows
- **Debounced Input**: Performance optimization for calculations
- **Inline Icons**: Visual indicators for numeric fields
- **Help Text**: Contextual guidance for complex numeric inputs

### Styling & Layout

- **Right Alignment**: Default right alignment for numeric values
- **Flexible Positioning**: Label placement options (above, left, hidden)
- **Field Modes**: Form input or inline editing styles
- **Responsive Design**: Configurable width and responsive behavior
- **Visual States**: Disabled, readonly, and dirty state indicators

### Advanced Features

- **Button Integration**: Calculator or action buttons
- **Conditional Logic**: Dynamic behavior based on other fields
- **Mathematical Operations**: Support for calculations and formulas
- **Accessibility**: Full keyboard navigation and screen reader support

## 📝 Usage Instructions

### Basic Setup

1. Add the Super Field - Number component to your form
2. Bind to a numeric data field
3. Configure label and placeholder text
4. Set validation rules (min/max values, required, etc.)

### Advanced Configuration

- **Formatting**: Use templates for currency or unit display
- **Validation**: Set up comprehensive numeric validation
- **Events**: Attach calculations or actions to value changes
- **Styling**: Customize alignment and positioning

### Common Use Cases

- **Financial Forms**: Currency amounts with proper formatting
- **Quantity Inputs**: Inventory counts and measurements
- **Calculations**: Age, percentages, and mathematical values
- **Data Entry**: Survey responses and statistical data

## 🔧 Configuration Options

| Setting        | Type     | Description                           |
| -------------- | -------- | ------------------------------------- |
| Field          | Number   | Numeric field binding                 |
| Label          | String   | Display label text                    |
| Placeholder    | String   | Input guidance text                   |
| Default Value  | Number   | Pre-filled numeric value              |
| Format Value   | Template | Display formatting template           |
| Help Text      | String   | Help/instruction text                 |
| Validation     | Rules    | Numeric validation (min/max/required) |
| Alignment      | Select   | Text alignment (left/center/right)    |
| Autofocus      | Boolean  | Auto-focus on load                    |
| Debounced      | Boolean  | Enable input debouncing               |
| Disabled       | Boolean  | Disable input interaction             |
| Read Only      | Boolean  | Read-only mode                        |
| Field Mode     | Select   | Form or inline input style            |
| Label Position | Select   | Label placement                       |
| Size           | Number   | Component width span                  |

## 📋 Events

### On Change

Triggered when the numeric value changes.

**Context:**

- `value`: The current numeric value
- `field`: The bound field information

## 🎨 Styling

Supports Budibase's styling system with:

- Size variants for different input scales
- Theming for financial vs. general numeric inputs
- Custom CSS for specialized number displays
- Responsive design for mobile forms

## 🔍 Best Practices

- Use right alignment for currency and measurement values
- Set appropriate min/max values to prevent invalid data
- Implement debouncing for calculation-heavy forms
- Provide clear units in labels or help text
- Consider locale formatting for international applications
- Use validation to ensure data integrity
