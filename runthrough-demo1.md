# Runthrough Demo - Build the app

## Create solution

We want to do things right and only work inside of a solution. Create a new solution, don't use the default publisher and off you go! 

## Create new Canvas App

- name it `Reiceipt Scanner`
- choose **Tablet** mode
- enable modern controls in **Settings**
- add **Analyze receipts**

In the App object

- **SizeBreakPoints**: `[450]`
- **OnStart**:

```
ClearCollect(
    colTrips,
    {id: GUID(), name: "London", start: Today()-50, end: Today()-45},
    {id: GUID(), name: "Buenos Aires", start: Today()-30, end: Today()-25},
    {id: GUID(), name: "New York", start: Today()-10, end: Today()-7},
    {id: GUID(), name: "Color Cloud Hamburg", start: Today(), end: Today()+1}
);

ClearCollect(
    colReceipts,
    // Trip 1: London
    {
        date: Today()-50,
        amount: 120.00,
        currency: "GBP",
        category: "travel",
        tripid: LookUp(colTrips, name = "London").id,
        company: "Airline Co."
    },
    {
        date: Today()-50,
        amount: 45.50,
        currency: "GBP",
        category: "food",
        tripid: LookUp(colTrips, name = "London").id,
        company: "Restaurant A"
    },
    {
        date: Today()-50,
        amount: 200.75,
        currency: "GBP",
        category: "accommodation",
        tripid: LookUp(colTrips, name = "London").id,
        company: "Hotel B"
    },
    {
        date: Today()-50,
        amount: 30.00,
        currency: "GBP",
        category: "miscellaneous",
        tripid: LookUp(colTrips, name = "London").id,
        company: "Store C"
    },
    // Trip 2: Buenos Aires
    {
        date: Today()-29,
        amount: 150.00,
        currency: "ARS",
        category: "travel",
        tripid: LookUp(colTrips, name = "Buenos Aires").id,
        company: "Train Co."
    },
    {
        date: Today()-29,
        amount: 60.00,
        currency: "ARS",
        category: "food",
        tripid: LookUp(colTrips, name = "Buenos Aires").id,
        company: "Cafe D"
    },
    {
        date: Today()-29,
        amount: 220.00,
        currency: "ARS",
        category: "accommodation",
        tripid: LookUp(colTrips, name = "Buenos Aires").id,
        company: "Hotel E"
    },
    {
        date: Today()-29,
        amount: 40.00,
        currency: "ARS",
        category: "miscellaneous",
        tripid: LookUp(colTrips, name = "Buenos Aires").id,
        company: "Store F"
    },
    // Trip 3: New York
    {
        date: Today()-9,
        amount: 180.00,
        currency: "USD",
        category: "travel",
        tripid: LookUp(colTrips, name = "New York").id,
        company: "Bus Co."
    },
    {
        date: Today()-9,
        amount: 55.00,
        currency: "USD",
        category: "food",
        tripid: LookUp(colTrips, name = "New York").id,
        company: "Restaurant G"
    },
    {
        date: Today()-9,
        amount: 250.00,
        currency: "USD",
        category: "accommodation",
        tripid: LookUp(colTrips, name = "New York").id,
        company: "Hotel H"
    },
    {
        date: Today()-9,
        amount: 35.00,
        currency: "USD",
        category: "miscellaneous",
        tripid: LookUp(colTrips, name = "New York").id,
        company: "Store I"
    },
    // Trip 4: Color Cloud Hamburg
    {
        date: Today(),
        amount: 170.00,
        currency: "EUR",
        category: "travel",
        tripid: LookUp(colTrips, name = "Color Cloud Hamburg").id,
        company: "Airline J"
    },
    {
        date: Today(),
        amount: 65.00,
        currency: "EUR",
        category: "food",
        tripid: LookUp(colTrips, name = "Color Cloud Hamburg").id,
        company: "Restaurant K"
    },
    {
        date: Today(),
        amount: 230.00,
        currency: "EUR",
        category: "accommodation",
        tripid: LookUp(colTrips, name = "Color Cloud Hamburg").id,
        company: "Hotel L"
    }
);

```

## Main Screen

(rename it!)

### con_mother (horizontal)

- **Padding**:  `16`, `16`, `16`, `16`
- **Gap**: `16`
- **Width**: `Parent.Width`
- **Height**: `Parent.Height`
- **X**: `0`
- **Y**: `0`
- **Dropshadow**: `Light`
- **Fill**: `RGBA(245, 245, 245, 1)`


  - con_header (horizontal) (💡srsly doesn't matter as we only have 1 control in that container)
      - Header
        - **Height**: `75`
        - **Show logo**: `false`
        - **Title**: `Add Receipts`
  - con_mobile (vertical)
        - **Padding**: `8`,`8`,`8`,`8`
        - **Height**: `80`
        - **Fill**: `RGBA(255, 255, 255, 1)`
        - **Border Radius**: `8`
        - **Visible**: `Mainscreen.Size=1` --> only visible on mobile
    - Textcanvas
      - **Text**: `"Select Trip"`
      - **Weight**: `'TextCanvas.Weight'.Semibold`
      - **AlignInContainer**: `Strech`
    - DropdownCanvas
      - **Items**: `AddColumns(colTrips, Text, $"{name} {Text(start, "dd.mm.")}-{Text(end, "dd.mm.yy")}")`
      - **OnChange**: `Set(gblTrip, Self.Selected.id)`
      - **DefaultSelectedItem**: `LookUp(AddColumns(colTrips, Text, $"{name} {Text(start, "dd.mm.")}-{Text(end, "dd.mm.yy")}"), id=gblTrip)`
      - **AlignInContainer**: `AlignInContainer.Stretch`
      - Edit Fields, Add **Text**
- con_bottom (horizontal)
  - **Padding**: `2`,`2`,`2`,`2`
  - **LayoutAlignItems**: `LayoutAlignItems.Stretch`
  - **Fill**: `RGBA(245, 245, 245, 1)`
  - **Gap**: `16`
    - con_sidebar (vertical)
      - **Padding**: `8`,`8`,`8`,`8`
      - **FillPortions**: `3`
      - **Border Radius**: `8`
        - TextCanvas
          - **Text***: `"Select Trip"`
          - **Weight**: 'TextCanvas.Weight'.Semibold
          - **AlignInContainer**: `AlignInContainer.Stretch`
        - Gallery (Title and Subtitle)
          - **Items**: `colTrips`
          - **AlignInContainer**: `AlignInContainer.Stretch`
          - **NextArrow**> **OnSelect**: `Set(gblTrip, ThisItem.id)`
          - **Title**> **Text**: `ThisItem.name`
          - **Subtitle** > **Text**: `$"{Text(ThisItem.start, "dd.mm.yyyy")}-{Text(ThisItem.end, "dd.mm.yyyy")}"`
    - con_main (vertical)
      - **FillPortions**: `7`
      - **Border Radius**: `8`
      - **AlignInContainer**: `AlignInContainer.Stretch`
      - **Padding**: `8`
      - **Fill**: `Color.White`
      - TextCanvas
        - **Text**: `"Receipts"`
        - **Weight**: `'TextCanvas.Weight'.Semibold`
        - **AlignInContainer**: `AlignInContainer.Stretch`
      - Toolbar
        - **AlignInContainer**: `AlignInContainer.Stretch`
        - **Items**:
   
```powerappsfl
  Filter(Table(
    {
        ItemKey: "new",
        ItemDisplayName: "New",
        ItemIconName: "Add",
        ItemAppearance: "Primary",
        ItemIconStyle: "Regular",
        ItemTooltip: "Add new item",
        visible: true
    },
    
    {
        ItemKey: "edit",
        ItemDisplayName: "Edit",
        ItemIconName: "Edit",
        ItemAppearance: "Subtle",
        ItemIconStyle: "Regular",
        visible: CountRows(Table1.SelectedItems)=1
    },
    {
        ItemKey: "delete",
        ItemDisplayName: "Delete",
        ItemIconName: "Delete",
        ItemDisabled: CountRows(Table1.SelectedItems)=0,
        ItemAppearance: "Subtle",
        ItemIconStyle: "Filled",
        visible: true
    },
    {
        ItemKey: "info",
        ItemDisplayName: "Info",
        ItemIconName: "Info",
        ItemAppearance: "Subtle",
        ItemIconStyle: "Regular",
        visible: true
    }
), visible)
```

- **OnSelect**:

```powerappsfl
Switch(
    Self.Selected.ItemKey,/* Action for Key 'new' */
    "new",
    Set(gblShowReceipt, true);
    Set(gblReceiptData, Blank());
    Reset(med_addMedia),/* Action for 'edit' */
    "edit",
    Notify("Edit clicked"),/* Action for 'delete' */
    "delete",
    Remove(colReceipts, Table1.SelectedItems),/* Action for 'info' */
    "info",
    Notify("Info clicked"),/* Default action */
    Notify("Unrecognized button clicked")
)
```

💡 Will give errors for the moment, don't worry!

- Table
  - **Items**: `Filter(colReceipts, tripid = gblTrip)`
  - Fields > Edit > **Add fields**: **date**, **category**, **company**, **amount** and **currency**
  - **EnableSorting**: "yes"
  - **EnableRangeSelection**: `'PowerAppsOneGrid.EnableRangeSelection'.Disable`
  - **EnableMultipleSelection**: `'PowerAppsOneGrid.EnableMultipleSelection'.Enable`
  - **AlignInContainer**: `AlignInContainer.Stretch`
  - **DateTimeFormat**: `'PowerAppsOneGrid.DateOnlyFormat'.ShortDate`
  - **ShowFooter**: `"yes"`

Above everything now at top level:

- con_overlay (horizontal)
  - **Width**: `Parent.Width`
  - **Height**: `Parent.Height`
  - **X**: `0`
  - **Y**: `0`
  - **LayoutJustifyContent**: `LayoutJustifyContent.Center`
  - **Fill**: `RGBA(0, 0, 0, 0.2)`
  - **Visible**: `gblShowReceipt`
    - con_receipt (vertical)
      - **Padding**: `12`,`12`,`12`,`12`
      - **AlignInContainer**: `AlignInContainer.Center`
      - **Height**: `480` - turn flexible Height `off`
      - **Width**: `250`
      - **Fill**: `RGBA(255, 255, 255, 1)`
      - **Gap**: `5`
        - ico_close
          - **Width**: `32`
          - **Height**: `Self. Width`
          - **AlignInContainer**: `AlignInContainer.End`
          - **OnSelect**: `Set(gblShowReceipt, false)`
        - TextCanvas
          - **Text**: `"Upload receipt"`
          - **Weight**: `'TextCanvas.Weight'.Semibold`
        - con_media
          - **Height**: `100` - turn flexible Height `off`
          - **LayoutMinWidth**: `150`
            - med_addMedia (add Picture control), ungroup
              - **Width**: `Parent.Width`
              - **Height**: `Parent.Height`
              - **Size**: `12`
              - **OnChange**:`Set(gblLoading, true);Set(gblReceiptData, 'Analyze receipt'.Predict(Self.Media).StructuredOutput); Set(gblLoading, false);`
            - con_receiptdata
            - **LayoutMinWidth**: `150`
            - **Padding**: `10`

        - con_receiptdata
            - **LayoutMinWidth**: `150`
            - **Padding**: `10`
            - **DropShadow**: `DropShadow`
              - TextCanvas
                - **AlignInContainer**: `AlignInContainer.Stretch`
                - **Text**: `"Company"`
                - **Height**: `20`
              - ⚠️ copy/paste this for **Amount**, **Currency**, and **Date**
              - txt_company
                - **AlignInContainer**: `AlignInContainer.Stretch`
                - **LayoutMinWidth**: `100`
                - **Value**: `gblReceiptData.company`
              - ⚠️ copy/paste this for **Currency** but adjust the value to `gblReceiptData.company`
            - num_amount
              - **AlignInContainer**: `AlignInContainer.Stretch`
              - **LayoutMinWidth**: `100`
              - **Value**: `gblReceiptData.amount`
            - date_date
            - **AlignInContainer**: `AlignInContainer.Stretch`
            - **LayoutMinWidth**: `100`
          - in con_receipt: Spinner
            - **Label**: `🦄🦄🦄`
            - **AlignInContainer**: `AlignInContainer.Stretch`
            - **Height**: `235`
            - **LabelPosition**: `'Spinner.LabelPosition'.Below`
            - **SpinnerSize**: `"Large"`
            - **Visible**: `gblLoading`
          - button
            - **AlignInContainer**: `AlignInContainer.Center`
            - **Text**: `Save`
            - **Icon**: `Save`
            - **DisplayMode**: `If(gblLoading, DisplayMode.Disabled, DisplayMode.Edit)`
            - **Onselect**:

```powerappsfl
Collect(
    colReceipts,
    {
        tripid: gblTrip,
        amount: num_amount.Value,
        company: txt_company.Value,
        currency: txt_currency.Value,
        date: date_date.SelectedDate
    }
);
Notify("Saved new receipt");
Set(
    gblShowReceipt,
    false
)
```
