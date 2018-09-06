# tcl_scripts
Creating a repository of all our TCL scripts presently deployed on CL-IDE
################################################
################################################
package provide hl7 1.0
namespace eval hl7 {
    # Globals
    global variable 123
    # Variables
    global env
    global segmentSep \r
    global fieldSep |
    global componentSep ^
    global subcomponentSep &
    global repetitionSep \~
    proc getData {msgData {segment ALL} {field -1} {component -1} {subcomponent -1} {repetition 1} } {
        # Set return data variable. 
        set returnValue ""    
        # Set a temp var for return to traverse each filter section
        set returnValueTemp ""
        # Split into a list of segments
        set segList [split $msgData $segmentSep]
        ##################################
        # STAGE 1: Filter segments
        ##################################
        set returnValueTemp $returnValue
        set returnValue ""
        # If segments parameter is not passed, fetch all segments as a list
        if { $segment == ALL } {
            set returnValueTemp $segList
        } else {
            # Get the index of the segment of interest
            set segmentIdx [lsearch -regexp -all $segList ^$segment]
            # If there are multiple segments of the kind, fetch all
            # and return as a list
            foreach idx $segmentIdx {
                lappend $returnValue [lindex $segList $idx]
            }
        }
        ##################################
        # STAGE 2: Filter fields
        ##################################
        set returnValueTemp $returnValue
        set returnValue ""
        if { $field > 0} {
            
            # Fetch the field for each segment into a list
            foreach item $returnValueTemp {
                set fieldList [split $item $fieldSep]
                lappend $returnValue [lindex $fieldList $field]
            }
        } else {
            set returnValue $returnValueTemp
        }
        ##################################
        # STAGE 3: Filter components
        ##################################
        set returnValueTemp $returnValue
        set returnValue ""
        if { $component > 0 } {
            # Fetch the components for each field into a list
            foreach item $returnValueTemp {
                set compList [split $item $componentSep]
                lappend $returnValue [lindex $compList $component]
            }
        } else {
            set returnValue $returnValueTemp
        }
        ##################################
        # STAGE 4: Filter subcomponents
        ##################################
        set returnValueTemp $returnValue
        set returnValue ""
        if { $subcomponent > 0 } {
            # Fetch the subcomponents for each field into a list
            foreach item $returnValueTemp {
                set subcompList [split $item $subcomponentSep]
                lappend $returnValue [lindex $subcompList $subcomponent]
            }
        }
        ##################################
        # STAGE 5: Filter repetitions
        ##################################
        set returnValueTemp $returnValue
        set returnValue ""
        if { $repetition > 1 } {
            # Fetch the subcomponents for each field into a list
            foreach item $returnValueTemp {
                set repList [split $item $repetitionSep]
                lappend $returnValue [lindex $repList $repetition]
            }
        } else if { $repetition == ALL } {
            # Fetch all the repetitions for each field into a list
            foreach item $returnValueTemp {
                set repList [split $item $repetitionSep]
                lappend $returnValue [lindex $repList $repetition]
            }
        } else {
            # Fetch the first repetition for each field into a list
            foreach item $returnValueTemp {
                set repList [split $item $repetitionSep]
                lappend $returnValue [lindex $repList 1]
            }
        }
        return returnValue
    }
}
