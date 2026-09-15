# KDS2.0 - Show status color horizontal line for each course in ticket Test Report

## Functional Testing

### 1. Status Color Horizontal Line Display Verification

#### 1.1. Horizontal Line Color Reflects Course Status

**Prerequisite(s):**
- A ticket with at least one course (Starters, Mains, or Desserts) containing items in different statuses (e.g., Fire, In Progress, Completed, Served).
- The KDS2.0 environment is accessible and the ticket is open in the ticket view.

**Step(s):**
1. Open a ticket that contains items in a course with mixed statuses.
2. Observe the horizontal line displayed above each course section.
3. Compare the color of the horizontal line with the status colors defined in the system configuration.

**Test Result(s):**

| Step | Expected Result | Actual Result |
|------|-----------------|---------------|
| 1 | Ticket view opens successfully with all courses displayed. | Pass |
| 2 | The horizontal line above each course is displayed in a color matching the latest status of the course (not grey). | Pass |
| 3 | The line color matches the configured status color (e.g., red for Fire, green for Completed). | Pass |

#### 1.2. Horizontal Line Thickness and Position

**Prerequisite(s):**
- A ticket with multiple courses, each containing at least one item.
- Status color feature is enabled in KDS2.0.

**Step(s):**
1. Open the ticket in the ticket view.
2. Inspect the horizontal line above each course header.
3. Verify the line thickness is 3 pixels (same as the previous grey line).
4. Verify the line position is directly above each course, separating it from the previous course.

**Test Result(s):**

| Step | Expected Result | Actual Result |
|------|-----------------|---------------|
| 1 | Ticket view opens with all courses visible. | Pass |
| 2 | A horizontal line is displayed above each course header. | Pass |
| 3 | Line thickness is 3 pixels, consistent with the prior grey line behavior. | Pass |
| 4 | Line is positioned above each course, separating it from the previous course. | Pass |

### 2. Earliest Status Priority Logic

#### 2.1. Earliest Status Determines Line Color

**Prerequisite(s):**
- A ticket with a course containing two or more items in different statuses.
- Item A is in Fire status, Item B is in Completed status within the same course (e.g., Starters).

**Step(s):**
1. Open the ticket with the Starters course containing Item A (Fire) and Item B (Completed).
2. Observe the horizontal line color above the Starters course.
3. Verify that the line color matches the earliest status of the course (Fire color), not the latest.

**Test Result(s):**

| Step | Expected Result | Actual Result |
|------|-----------------|---------------|
| 1 | Ticket opens with Starters course showing Item A (Fire) and Item B (Completed). | Pass |
| 2 | Horizontal line above Starters is displayed in Fire color (e.g., red). | Pass |
| 3 | Line color follows the earliest status within the course (Fire), consistent with ticket status behavior. | Pass |

#### 2.2. Status Priority Across Multiple Items

**Prerequisite(s):**
- A ticket with a course containing three items:
  - Item A: In Progress status
  - Item B: Fire status
  - Item C: Completed status

**Step(s):**
1. Open the ticket with the course containing the three items with statuses In Progress, Fire, and Completed.
2. Observe the horizontal line color above the course.
3. Verify the line color matches the earliest status among the items (Fire status).

**Test Result(s):**

| Step | Expected Result | Actual Result |
|------|-----------------|---------------|
| 1 | Ticket opens with the course showing the three items in specified statuses. | Pass |
| 2 | Horizontal line is displayed in Fire color (the earliest status). | Pass |
| 3 | Line color reflects the earliest status across all items in the course, not the latest or mixed. | Pass |

### 3. Multi-Course Ticket Layout

#### 3.1. Independent Line Color Per Course

**Prerequisite(s):**
- A ticket with three courses: Starters, Mains, and Desserts.
  - Starters: all items in Fire status
  - Mains: all items in Completed status
  - Desserts: all items in Served status

**Step(s):**
1. Open the ticket with the three courses in different statuses.
2. Observe the horizontal line above each course.
3. Verify each course's line color is independent and matches the status of that course only.

**Test Result(s):**

| Step | Expected Result | Actual Result |
|------|-----------------|---------------|
| 1 | Ticket opens with Starters, Mains, and Desserts visible. | Pass |
| 2 | Starters line is in Fire color, Mains line is in Completed color, Desserts line is in Served color. | Pass |
| 3 | Each course line color is independent and does not inherit the status of other courses. | Pass |

#### 3.2. Course Separation and Visual Consistency

**Prerequisite(s):**
- A ticket with multiple courses, each with varying numbers of items (1 item in Starters, 5 items in Mains, 2 items in Desserts).

**Step(s):**
1. Open the ticket with the multi-course layout.
2. Verify the horizontal line appears above each course section.
3. Confirm the line visually separates each course from the previous one.
4. Verify the line color per course matches the earliest status of items within that course.

**Test Result(s):**

| Step | Expected Result | Actual Result |
|------|-----------------|---------------|
| 1 | Ticket opens with all three courses visible. | Pass |
| 2 | A horizontal line appears above Starters, Mains, and Desserts. | Pass |
| 3 | Each course is visually separated from the previous course by the line. | Pass |
| 4 | Line color per course matches the earliest status of items within that course. | Pass |

### 4. Edge Cases

#### 4.1. Empty Course

**Prerequisite(s):**
- A ticket with a course (e.g., Mains) that has no items assigned to it.

**Step(s):**
1. Open the ticket with the empty Mains course.
2. Observe whether a horizontal line is displayed above the empty course.
3. Verify the line color (if displayed) defaults to a neutral color (e.g., grey) or no line is shown.

**Test Result(s):**

| Step | Expected Result | Actual Result |
|------|-----------------|---------------|
| 1 | Ticket opens with the empty Mains course visible. | Pass |
| 2 | A horizontal line is either not displayed or shown in the default grey color for the empty course. | Pass |
| 3 | No status color is applied to an empty course since there is no status to reflect. | Pass |

#### 4.2. All Items in the Same Status

**Prerequisite(s):**
- A ticket with a course (e.g., Starters) where all items are in Completed status.

**Step(s):**
1. Open the ticket with the Starters course where all items are Completed.
2. Observe the horizontal line color above the Starters course.
3. Verify the line color matches the Completed status color.

**Test Result(s):**

| Step | Expected Result | Actual Result |
|------|-----------------|---------------|
| 1 | Ticket opens with Starters course showing all items in Completed status. | Pass |
| 2 | Horizontal line is displayed in Completed status color (e.g., green). | Pass |
| 3 | Line color matches the single status since all items share the same status. | Pass |

#### 4.3. Single Item in a Course

**Prerequisite(s):**
- A ticket with a course (e.g., Desserts) containing only one item in Fire status.

**Step(s):**
1. Open the ticket with the Desserts course containing one item in Fire status.
2. Observe the horizontal line color above the Desserts course.
3. Verify the line color matches the Fire status color.

**Test Result(s):**

| Step | Expected Result | Actual Result |
|------|-----------------|---------------|
| 1 | Ticket opens with Desserts course showing one item in Fire status. | Pass |
| 2 | Horizontal line is displayed in Fire status color. | Pass |
| 3 | Line color reflects the status of the single item correctly. | Pass |

#### 4.4. Status Change Updates Line Color

**Prerequisite(s):**
- A ticket with a Starters course containing two items:
  - Item A: Fire status
  - Item B: In Progress status
- The line color is initially Fire (the earliest status).

**Step(s):**
1. Open the ticket and observe the initial line color above Starters (Fire color).
2. Change Item A's status from Fire to Completed.
3. Observe the horizontal line color after the status change.
4. Verify the line color updates to reflect the new earliest status (In Progress color).

**Test Result(s):**

| Step | Expected Result | Actual Result |
|------|-----------------|---------------|
| 1 | Initial line color is Fire (red), reflecting the earliest status. | Pass |
| 2 | Item A status is changed to Completed successfully. | Pass |
| 3 | Line color updates to In Progress color (the new earliest status after the change). | Pass |
| 4 | Line color dynamically updates to reflect the earliest status after any status change within the course. | Pass |
