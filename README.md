# QField_Geological_mapping_template
A QGIS template project for geological mapping using QField

QField is a portable version of QGIS. I am using it for geological mapping, using a suite of tools to tweak the functionalities to the specific needs of this activity:
- A QGIs project and database template ([you are here!](https://github.com/jfmoyen/QField_Geological_mapping_template/));
- A plugin adding a geological compass to measure and record planes and lines ([here](https://github.com/jfmoyen/QField_qml_Geological_compass)), adapted from [Mark Jessel's work](https://github.com/swaxi/compass);
- A plugin streamlining the use of camera ([here](https://github.com/jfmoyen/QField_MySnap)), modified from [QField plugin Snap!](https://github.com/opengisch/qfield-snap)

  The three tools can be used independently. Together, they offer a platform that works well (at least, for me) in the field. A source of inspiration for this project is the [FieldMove]((https://www.petex.com/products/move-suite/digital-field-mapping/)) application: the first goal was to reproduce FieldMove's functionalities, but adding the flexibility and "openness" of QField.

  # A database and a QGIS project for geological mapping

  This project provides presets to help recording field data. It taps into several lesser known functionalities of QGIS, such as [customized forms](https://docs.qgis.org/3.44/en/docs/training_manual/create_vector_data/forms.html)), joins and advanced symbology to interact with a field database. Of course, the files supplied here are only a template - you should customize them according to your own needs. 

  ## The database
  The database, stored as a geopackage, which ensures better data integrity, and allows to move everything as one single file. Historically, the table fields are inherited from FieldMove (and largely remain compatible with FM's tables), which is why they have a series of probably unneeded fields.
  
   The database is made of 6 tables. Four of them have geometries and store actual field data, two are geoemtry-less reference tables.

  ### Data tables
- **Localities** records individual sites visited during field work. Typically a site would correspond to an entry in your field notes. The concept is more relevant for "spot" work, driving or walking between independent outcrops than proper mapping.
In the database, *localities* are used as a key identifier to which individual samples, photos or structural readings are attached.

- **Samples** (points) for, obvisiously, samples that you collect. Samples are attached to a *locality*, and they have a *lithology*.

- **Sketching** allows to record (line) geometries, similar to drawing on a basemap. A line is tied to a *lithology*, and additionally you can define its thickness. You can also record its meaning (observed, inferred, etc).

- **Structural readings** (points) is the most complex table. One record corresponds to one (compass) reading; each is individually located and associated to plane and line orientation, as well as its meaning and lithology. Readings have several properties:
  -  Metatadata such as X, Y, date, time... 
  -  Shape, nature, meaning (see below *Structure types*) records what you are measuring (a bedding, a fault plane...). `ObjectGeom` records if the reading concerns a plane, a line or both; `ObjectType` describes if the orienation is penetrative, discrete, etc. and `ObjectNature` describes what the object actually is (see below)
  -  *Lithology*;
  -  Orientation for planar and/or linear data
     -  Planes have fields for strike (`P_strike`), dip direction (`P_dipAzimuth`) and dip (`P_dip`). Clearly strike and dip direction are redundant. The QGS project (and the [Geological Compass](https://github.com/jfmoyen/QField_qml_Geological_compass) plugin) fill both.
     -  Lines have fields for trend (`L_plungeAzimuth`) and plunge (`L_plunge`). They also have a pitch. The QGS project and the plugin will calculate the pitch of a line attached to a plane.

- **Photos**, as the name implies, records photos. The most important field, `image_path`, records the path to the photo file, normally under `DCIM/` (which is where QField stores photos by default)

### Reference tables
 Two geometry-less tables provide "dictionaries" that are used in the attribute forms of the four tables above. These are the tables you should edit if you want, for instance, to add a new lithology (this cannot be done directly from the other tables).
 
  -  **rock_units** defines the lithologies you are working with. Each lithology is defined by a `name` and a `color` (that will be used in the symbology). The boolean field `Display` allows to "hide" some of the rocks from the dropdown menus, to unclutter them during field work. To add new lithologies, add new lines to this table - do not forget to set `Display` to `true`.

  -  **Structure_types** stores a list of possible values and combination of geometries, types and natures for structural readings.
     - *Geometry* (field `ObjectGeom`): are we recording a plane (e.g. a bedding, a fault...), a line (a lineation), both together (a fault with slickenslides, a foliation with lineation) or none at all (useful for mapping when you are just trying to record a rock occurence to constrain a contact)?
     - *Type* of object (field `ObjectType`): a reading could be for instance the main rock fabric (what you will typically show on your map with a dip symbol, which generally will follow the contacts), a discrete structure, the wall of a dyke, etc.
     - *Nature* of the object (field `ObjectNature`): more precisions. For instance a rock fabric could be a foliation, a bedding, and overturned bedding... probably they will have different symbols.

By editing this table, you can add more types. You could, for instance, add a new type of planar fabric to record a second cleavage (S2) as follows:

| Field| Value |
| --- | --- |
| ObjectGeom  | Plane    | 
| ObjectType | Rock fabric   | 
| ObjectNature    | S2   |

  ## The QGIS project
  The .qgs project links the different tables and provides attribute forms and symbology. It should be regarded as a base for your work -- you should add more basemaps, perhaps detailed imagery, existing sample sets, etc. 
  - **Linking** of tables is made with *joins*. For instance, the color of every feature is defined by its lithology, and taken from the color defined in *rock_units*. If you change the color there, it will be automatically mirrored elsewhere.
  - **Input forms** simplify the data input in the field. All lithology fields use drop-down lists (populated from the *rock_units* table). Tables mostly have sensible defaults for things like date/time, year, locality (the nearest point from the *localities* layer at the time of feature creation, meaning that you should, generally speaking, first create a locality before recording anything else). The most complex form is probably the one for *structural_readings*: strike will be calculated automatically from dip drection, pitch will be calculated from the other values, line or plane geometries will be hidden as a function of the geometry type, etc.
  - **Symbology** takes into account these properties. Color is defined based on the lithology, etc. Here too, the most detailed symbology is for structural readings, as it included rotation of symbols, colouring, different symbols for different types of structures, etc. You should probably customize this symbology based on your own needs! The (svg) symbols used are integrated in the project, to keep everything portable.
    
