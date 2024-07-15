- The top and bottom margins of blocks are sometimes combined (collapsed) into a single margin whose size is the largest of the individual margins (or just one of them, if they are equal)
- the margins of floating and absolutely positioned elements never collapse.
- Margin collapsing occurs in three basic cases:
    - Adjacent siblings
        - The margins of adjacent siblings are collapsed (except when the latter sibling needs to be cleared past floats).
    - Parent and first/last child
        - If there is no border, padding, inline part, block formatting context created, or clearanceto separate the margin-top of a block from the margin-top of its first child block; or no border, padding, inline content, height, min-height, or max-height to separate the margin-bottom of a block from the margin-bottom of its last child, then those margins collapse. The collapsed margin ends up outside the parent.
    - Empty blocks
        - If there is no border, padding, inline content, height, or min-height to separate a block's margin-top from its margin-bottom, then its top and bottom margins collapse.
- More complex margin collapsing (of more than two margins) occurs when the above cases are combined.
- These rules apply even to margins that are zero
    - the margin of a first/last child ends up outside its parent (according to the rules above) whether or not the parent's margin is zero.
- When negative margins are involved, the size of the collapsed margin is the sum of the largest positive margin and the smallest (most negative) negative margin.
- When all margins are negative, the size of the collapsed margin is the smallest (most negative) margin.
    - This applies to both adjacent elements and nested elements.
- Dealing with margin collapsing
    - knowing about it and dealing with it as it comes up
    - systematically flowing margins in one direction

[[Margins (CSS)]]