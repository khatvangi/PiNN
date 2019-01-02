Datasets
========

General
-------
PiNN use tensorflow's dataset class to handel data. Each data point
is a dictionary of tensors. The data point should at least include
the ``'coord': coordinates`` and ``'atoms': atomic numbers`` as its
features. For training potentials, ``'e_data': energy`` and optionally
``'f_data': forces`` are used as labels.

For performance reasons, the data points are batched in a dense form.
This means all data points in one dataset should have the same number
of atoms, with empty atoms or coordinates padded by zeros. The
potential model assumes that the dataset is batched, meaning the shapes
of the tensors should be
``'coord'/'f_data': [nbatch, natoms, 3]``
``'atoms':[nbatch, n],``
``'e_data': [nbatch]``

Splitting the dataset
---------------------
It is common practice to split the dataset into subsets for validation
in ML tasks. Our dataset loaders support a ``split_ratio`` option to
do this, the split_ratio can be a nested dictionary of relative ratios
of subsets (e.g. ``{train:8, test:[1,2,3]}``). The dataset loader will
return a nested structure of datasets with corresponding ratios.

By default, the dataset are splitted into three subsets (train: 80%,
test: 10%, vali: 10%)


Numpy dataset
-------------
The easist way to generate you own dataset is to store the data as a
dictionary of numpy arrays. See how it's done in the
:doc:`toy problem <notebooks/Toy_LJ_with_three_atoms>`.

TFRecord dataset
----------------
For larger datasets which do not fit in the memory, or for training
on Google Cloud, it's more efficient to store the data in the
tfrecord format. (The converter and loader of TFRecord datasets are
under development)


Parsers for common datasets
---------------------------
For common QML datasets, we provide the function to directly load
the dataset. The loaders will be limited by IO, but if you have enough
memory, you can simply cache the dataset with ``dataset.cache()``.

QM9 dataset
...........
The QM9 dataset (https://doi.org/10.6084/m9.figshare.978904) consists
of 134K organic molecules. The QM9 dataset include many properties,
however, our dataset load only the enthalpy at this moment.


ANI-1 dataset
.............
The ANI-1 dataset (https://doi.org/10.6084/m9.figshare.c.3846712.v1)
consists of 20M off-equilibrium DFT energies for orginic molecules.
(ANI-1 dataset parser is currently under development)