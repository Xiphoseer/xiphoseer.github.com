---
title: SGML Recognition Modes
author: Xiphoseer
---

I'm currently looking into the differences between SGML and XML and
as it turns out, SGML defines different "recognition modes" to easily
find the relevant "delimiter characters" for the upcoming markup.

I'll list them here with the relevant delimiters, which is inverse from
the table in the SGML handbook, but lends itself better to a parser
implementation from my point of view.

## Content (CON)

This is the mode the parser starts out in.

- CRO `&#`: Character reference open (CREF)
- ERO `&`: Entity reference open (NMS)
- ETAGO `</`: End-tag open
- MDO `<!`: Markup declaration open (DCL)
- MSC `]]`: Marked section close (MSE: dsc i.e. followed by `>`)
- NET `/`: Null end-tag (ELEM only)
- PIO `<?`: Processing instruction open
- STAGO `<`: Start tag open (GI)

## Contextual Sequence (CXT)

- COM `--`: Comment start or end
- DSO `[`: Delimiter subset open
- GRPO `(`: Group open
- MDC `>`: Markup declaration close
- TAGC `>`: Tag close

## Declaration Subset (DS)

- DSC `]`: Delimiter subset close (ENT)

## Declaration Subset incl. Marked Section (DSM)

- MDO `<!`: Markup declaration open (DCL)
- MSC `]]`: Marked section close (MSE: dsc i.e. followed by `>`)
- PERO `%`: Parameter entity reference open
- PIO `<?`: Processing instruction open

## Group (GRP)

- AND `&`: And connector
- DTGC `]`: Data tag group close
- DTGO `[`: Data tag group open
- GRPC `)`: Group close
- GRPO `(`: Group open
- LIT `"`: Literal start or end
- LITA `'`: Literal start or end (alternative)
- OPT `?`: Optional occurence indicator
- OR `|`: Or connector
- PERO `%`: Parameter entity reference open (NMS)
- PLUS `+`: Required and repeatable, inclusion
- REP `*`: Optional and repeatable
- RNI `#`: Reserved name indicator
- SEQ `,`: Sequence connector

## Literal (LIT)

- CRO `&#`: Character reference open (CREF)
- ERO `&`: Entity reference open (NMS)
- LIT `"`: Literal start or end
- LITA `'`: Literal start or end (alternative)
- PERO `%`: Parameter entity reference open (NMS)

## Markup Declaration (MD)

- COM `--`: Comment start or end
- DSC `]`: Delimiter subset close (ENT)
- DSO `[`: Delimiter subset open
- GRPO `(`: Group open
- LIT `"`: Literal start or end
- LITA `'`: Literal start or end (alternative)
- MDC `>`: Markup declaration close
- MINUS `-`: Exclusion (EX: i.e. before grpo)
- PERO `%`: Parameter entity reference open (NMS)
- PLUS `+`: Required and repeatable, inclusion (EX: i.e. before grpo)
- RNI `#`: Reserved name indicator

## Processing Instruction (PI)

- PIC `>`: Processing instruction close

## Reference (REF)

- REF `;`: Reference close

## Start- or End-Tag (TAG)

- ETAGO `</`: End-tag open (GI)
- LIT `"`: Literal start or end
- LITA `'`: Literal start or end (alternative)
- NET `/`: Null end-tag
- STAGO `<`: Start tag open (GI)
- TAGC `>`: Tag close
- VI `=`: Value indicator
