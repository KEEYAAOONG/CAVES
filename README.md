# CAVES (Canonical Abbreviation for Vector-Encoded Sites)
A proposal for generating canonical, string-based descriptors for protein binding pockets.

## Motivation

Current ligand discovery pipelines rely heavily on brute-force screening due to the absence of a standardized, searchable representation of binding pockets. While ligands can be described with SMILES strings, no analogous, compact representation exists for protein binding sites.

If binding pockets could be encoded as deterministic, linear strings (like SMILES), they could be:

Compared, clustered, and indexed across databases

Used to learn mappings to preferred ligand features

Enable predictive modeling of ligand compatibility

This would enable large-scale matching between known ligand libraries and existing pockets, enhancing drug discovery pipelines.

## Challenges

Some proteins have multiple binding pockets, which complicates representation.

Undruggable targets may lack well-defined binding pockets.

Pocket definition can vary depending on ligand conformation or detection method.

The CAVES format aims to offer a balance of structure, interpretability, and machine learning compatibility for encoding binding pockets into symbolic descriptors.

Foundational Assumptions, Hypotheses, and Constraints

## Hypotheses

H1: Binding site functionality can be abstracted using structural and physicochemical patterns.

H2: Binding pockets can be consistently encoded into linear strings using defined canonical rules.

H3: These strings are sufficiently informative to support downstream ML applications such as ligand prediction.

## Design Constraints

C1: Output must be deterministic — the same pocket always yields the same string.

C2: Encodings must be interpretable and reconstructable at a symbolic level.

C3: Strings should be compact and comparable.

C4: Inter-residue relationships must be preserved through ordering or annotation.

## Structural Assumptions

A1: Binding pockets can be defined via residue-level data from structure-based tools (e.g. fpocket).

A2: Each residue in a pocket can be described by local physicochemical features.

A3: A geometric centroid exists from which canonical ordering can be derived.

A4: Multi-pocket proteins are handled as multiple independent entries.

A5: Targets with no defined pocket are excluded or marked as non-encodable.

## Design Goals

Interpretability: Human-readable symbolic abstraction

Compactness: Concise yet feature-rich

Machine-Friendliness: Tokenizable, embeddable, vector-ready

Uniqueness: Distinct strings for distinct pockets

### Step 1: Define the Unit

Unit of encoding: Residue

Each residue in the binding site is treated as a unit.

Residue-level abstraction allows human readability and reduces complexity compared to atom-level encoding.

### Step 2: Annotate Each Unit

Each residue is annotated with relevant features:

Residue type: e.g. GLU, HIS, ASP

Position index: e.g. 45, 57, 102

Functional role: e.g. hydrogen bond donor/acceptor, aromatic

Charge: Positive, Negative, Neutral

Hydrophobicity: High, Medium, Low

Exposure: Surface-exposed, buried

Center coordinates: (x, y, z)

Example:

GLU45 → [Negative, HB-acceptor, surface-exposed, center=(4.1, 2.3, 7.0)]

### Step 3: Canonicalization (Ordering)

Residues are ordered based on a canonical rule:

Distance from pocket centroid (closest first)

If distance is equal, sort lexicographically by residue name or index

Goal: Ensure the same pocket yields the same string regardless of input order.

### Step 4: Tokenization

Residues are converted into symbolic tokens:

Each token encodes residue type and key features

Examples:

GLU (Negative) → G-
HIS (Aromatic) → H+
ASP (Negative) → D-

Tokens are then concatenated based on canonical order:

G--H+-D-

### Step 5: Serialization

Final string encodes pocket shape + features in compressed form:

G--H+-D-[radius=4.2Å][hydrophobicity=low][HBD=2]

Optional compression: base64 or custom character mapping.

Properties:

Compact

Deterministic

Comparable

## Future Considerations

Embed 3D shape hash

Train pocket embedding model to reverse-engineer this

Explore mutation-invariant encoding

CAVES: A structured, symbolic identity for your vector-encoded binding site.
