# Remove URIs from VIVO

Given a list of URIs in VIVO, remove them and all statements that
have the specified URIs as either subject or object.

# Note

NOTE: Use of remove_uri may create data integrity problems in your VIVO
triple store.  remove_uri is provided as a means for quickly removing
large numbers of URIs and their related assertions.  It does just that.
It should be used as part of a comprehensive data management activity
to identify and correct data integrity problems in a VIVO triple store.

# What is removed

Suppose you have a URI "x".  remove_uri will remove all statements from VIVO
that match

    x predicate object
	
and all statements that match

    subject predicate x
	
where subject is *any* subject, predicate is *any* predicate and object is *any* object.

DANGER: There is no limit to the number of statements that may be removed.  If your VIVO is
all statements about "x" and you say remove x, it will remove all the statements.

# How to use remove_uri responsibly

Despite the danger, remove_uri is very handy and can be used responsibly.  Here's a process
that you can use to remove uris without *too much* danger:

1.  Use a query to generate a list of the URIs you want to remove.  Using a query is a good idea --
you can test the query, look at its results and understand why and what uris are going to be removed.
1. Put the query results in a file for use be remove_uri.  The file must have a column named "uri".
1. Run remove_uri
1. Inspect the results
    1. Inspect the log results.  In the log, remove_uri shows each URI to be removed and the amount of
       RDF (in characters) that it will take to remove the URI.  
	1. Check these results by inspecting
       the objects on the the list in VIVO. 
	1. Check these results by using the [VIVO Triple Inspector]() to see the
       actual statements for each URI.  The Triple inspector only shows the triples of the form x predicate object.
	   It does not show statements where the specified URI is used as an object.
	1. Inspect the RDF that will be subtracted from VIVO.  All the statements should be ones that you wish to remove
	   from VIVO.
1. If you are uncertain about what statements will be removed, or whether the statements are the statements
that should be removed, *STOP!*
1.  If you are *sure* these are the statements you want to remove, then go then admin menu, select add/remove RDF,
select the sub.rdf, click on remove RDF and press submit.  The statements will be removed.
1.  Save the sub.rdf file in a safe place in case you made a mistake and need to restore.
    1. If you made a horrendous mistake, you can take the sub.rdf file produced by remove_uri and add it back into VIVO, reversing the remove process.
	
Following the steps above, you should be able to use remove_uri and stay safe.

# Some uses for remove_uri

Why do we need such a tool?  Here are some examples of uses:

1.  Remove people who should not be in VIVO.  You may have people in your VIVO that you would like to remove.
    If you can list their URIs, you can remove them.
1.  Clean up data integrity problems in VIVO.  You may have positions without people in them.  You may have
    authorships or roles that do not point to entities as they should.  You may have dates that are unused.
    If you can list the URIs of the entities to be removed, remove_uri can provide the RDF necessary to 
    remove them.

# A final note

remove_uri may *create* data integrity problems.  If you remove person x, and x is in an authorship,
remove_uri will remove the statement "x in authorship y" as well as the statement "authorship has linked
author x".  There's no magic here.  remove_uri simply removes statements matching the patterns described.  
This *will* result in broken authorships.  A subsequent remove_uri run can be used to remove authorships
that do not have people associated with them.  to repeat what was said earlier:  remove_uri should be used as part of a comprehensive data management activity
to identify and correct data integrity problems in a VIVO triple store.	
