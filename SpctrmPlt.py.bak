# This module contains the functions needed to plot multiple optical spectra on the same graph
# This module is necessary because the same type of plot is required very often
# R. Sheehan 26 - 2 - 2018

# Necessary imports
import os
import glob
import re
import sys # access system routines

import math
import scipy
import numpy as np
import matplotlib.pyplot as plt

import Common
import Plotting

def multiple_optical_spectrum_plot(dir_name, file_names, labels, plot_range, plt_title = '', plt_name = '', loudness = False):
    # make a plot of a set of measured optical spectra
    # R. Sheehan 30 - 8 - 2017

    try:

        HOME = os.getcwd() # Get current directory

        if os.path.isdir(dir_name): # check if dir_name is valid directory
            os.chdir(dir_name) # if dir_name is valid location move there

            # test inputs for validity
            c1 = True if file_names is not None else False
            c2 = True if labels is not None else False
            c3 = True if len(file_names) == len(labels) else False
            c4 = True if plot_range is not None else False
            c5 = True if len(plot_range) == 4 else False
            c6 = True if c1 and c2 and c3 and c4 and c5 else False

            if c6:
                delim = '\t' # should include something to check what delimiter is from the file being read
                hv_data = []; mark_list = []

                for i in range(0, len(file_names), 1):
                    if glob.glob(file_names[i]): # ensure that file in list exists
                        data = Common.read_matrix(file_names[i], delim)
                        data = Common.transpose_multi_col(data)
                        hv_data.append(data); 
                        mark_list.append(Plotting.labs_lins[i%len(Plotting.labs_lins)]);
                    else:
                        # this will raise an exception below
                        print "\nError: SpctrmPlt.multiple_optical_spectrum_plot()\nCould not locate:",file_names[i]

                # Need to have number of data sets equal to number of labels for plotting methods to work                
                if len(hv_data) == len(labels):
                    arguments = Plotting.plot_arg_multiple()

                    arguments.loud = loudness
                    arguments.x_label = 'Wavelength (nm)'
                    arguments.y_label = 'Spectral Power (dBm/0.05 nm)'
                    arguments.plt_range = plot_range
                    arguments.crv_lab_list = labels
                    arguments.mrk_list = mark_list
                    arguments.plt_title = plt_title
                    arguments.fig_name = plt_name

                    Plotting.plot_multiple_curves(hv_data, arguments)

                    del hv_data; del mark_list;
                else:
                    raise Exception
                os.chdir(HOME)
            else:
                raise Exception
        else:
            raise EnvironmentError
    except EnvironmentError:
        print "\nError: SpctrmPlt.multiple_optical_spectrum_plot()"
        print "Error: Could not find",dir_name
    except Exception:
        print "\nError: SpctrmPlt.multiple_optical_spectrum_plot()"
        if c1 == False: print "dir_names not assigned correctly"
        if c2 == False: print "labels not assigned correctly"
        if c3 == False: print "dir_names and labels have different lengths"
        if c4 == False: print "range not assigned correctly"
        if c5 == False: print "range does not have correct length"