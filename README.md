# Methods

## add\_form\_invalid

## validation\_errors\_to\_html

## refill\_form

# DESCRIPTION

Form-validation using a Catalyst controller and Data::FormValidator

# SYNOPSIS

## Form Validation

    use base 'Catalyst::Controller::Validation::DFV';
    use Data::FormValidator::Constraints qw(:closures);

    # define a DFV profile
    my $dfv_profile = {
        required => [qw<
            email_address
            phone_home
            phone_mobile
        >],

        constraint_methods => {
            email_address   => email(),
            phone_home      => american_phone(),
            phone_mobile    => american_phone(),
        },
    };

    # check the form for errors
    $c->forward('form_check', [$dfv_profile]);

    # perform custom/complex checking and
    # add to form validation failures
    if (not is_complex_test_ok()) {
        $c->forward(
            'add_form_invalid',
            [ $error_key, $error_constraint_name ]
        );
    }

## Form Refilling

    package MyApp::Controller::Root;

    # ...

    use base 'Catalyst::Controller::Validation::DFV';

    # ...

    sub render : ActionClass('RenderView') {
        # ...
    }

    sub end : Private {
        my ($self, $c) = @_;

        # render the page
        $c->forward('render');

        # fill in any forms
        $c->forward('refill_form');
    }

# EXAMPLES

There are [Template::Toolkit](https://metacpan.org/pod/Template%3A%3AToolkit) file examples in the examples/ directory of
this distribution.

# AUTHOR

Chisel <chisel@chizography.net>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2012 by Chisel Wright.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
