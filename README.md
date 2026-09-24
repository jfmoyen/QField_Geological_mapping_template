# QField_Geological_mapping_template
A QGIS template project for geological mapping using QField

QField is a portable version of QGIS. I am using it for geological mapping, using a suite of tools to tweak the functionalities to the specific needs of this activity:
- A QGIs project and database template (this repo);
- A plugin adding a geological compass to measure and record planes and lines (here);
- A plugin streamlining the use of camera (there)

  The three tools can be used independently. Together, they offer a platform that works well (at least, for me) in the field. A source of inspiration for this project is the FieldMove application: the first goal was to reproduce FieldMove's functionalities, but adding the flexibility and "openness" of QField.

  # A database and a QGIS project for geological mapping

  This project provides presets to help recording field data. It taps into several lesser known functionalities of QGIS, such as customized forms, joins and advanced symbology to interact with a field database. Of course, the files supplied here are only a template - you should customize them according to your own needs. 

  ## The database
  The database, stored as a geopackage, which ensures better data integrity, and allows to move everything as one single file. Historically, the table fields are inherited from FieldMove (and largely remain compatible with FM's tables), which is why they have a series of probably unneeded fields.
  
   The database is made of 6 tables:

  ### Data tables
  - **Localities** records individual sites visited during field work. Typically a site would correspond to an entry in your field notes. The concept is more relevant for "spot" work, driving or walking between independent outcrops than proper mapping.
    In the database, *localities* are used as a key identifier to which individual samples, photos or structural readings are attached.
  


- **Structural readings** is the most complex table. One record corresponds to one (compass) reading; each is individually located and associated to plane and line orientation, as well as its meaning and lithology. Readings have several properties:
-  Their *shape* (field Geometry): are we recording a plane (e.g. a bedding, a fault...), a line (a lineation), both together (a fault with slickenslides, a foliation with lineation) or none at all (useful for mapping when you are just trying to record a rock occurence to constrain a contact)?
  - A *meaning* (field Meaning): a reading could be for instance the main rock fabric (what you will typically show on your map with a dip symbol, which generally will follow the contacts), a structure, a distinct dyke, etc.
  - a *nature* (field Nature): more precisions. For instance a rock fabric could be a foliation, a bedding, and overturned bedding...



- Lithology
- Various metadata such as X, Y, time of reading, etc.
- The actual reading

- - **Sketching** allows to record line geometries, corresponding to drawing on a basemap. A line is tied to a lithology, and additionally you can define its thickness. You can also record its meaning (observed, inferred, etc).
 
  - **Samples** for, obvisiously, samples that you collect. Samples are attached to a locality, and they have a lithology.

### Reference tables
 Two geometry-less tables provide "dictionaries" that are used in the attribute forms of the four tables above. These are the tables you should edit if you want, for instance, to add a new lithology (this cannot be done directly from the other tables).
 
  -  **Rock_units**
  -  **Structure_types** 

A list of possible values and combination is stored in the table Structure_types. By editing this table, you can add more types. You could, for instance, add a new type of planar fabric to record a second cleavage (S2) as follows:
| Field     | Value | 
| -------- | ------- | 
| ObjectGeom  | Plane    | 
| ObjectType | Rock fabric   | 
| ObjectNature    | S2   |

  ## The QGIS project
  The .qgs project links the different tables and provides attribute forms and symbology. It should be regarded as a base for your work -- you should add, for instance, more basemaps, perhaps detailed imagery, etc. 
  - **Linking** of tables is made with *joins*. For instance, the color of every feature is defined by its lithology, and taken from the color defined in *rock_units*. If you change the color there, it will be automatically mirrored elsewhere.
  - **Input forms** simplify the data input in the field. All lithology fields use drop-down lists (populated from the rock_units table). Tables mostly have sensible defaults for things like date/time, year, locality (the nearest point from the *localities* layer at the time of feature creation, meaning that you should, generally speaking, first create a locality before recording anything else). The most complex form is probably the one for *structural_readings*: strike will be calculated automatically from dip drection, pitch will be calculated from the other values, line or plane geometries will be hidden as a function of the geometry type, etc.
  - **Symbology** takes into account these properties. Color is defined based on the lithology, etc. Here too, the most detailed symbology is for structural readings, as it included rotation of symbols, colouring, different symbols for different types of structures, etc. You should probably customize this symbology based on your own needs! The (svg) symbols used are integrated in the project, to keep everything portable.
    
