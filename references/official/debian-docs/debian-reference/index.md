---
title: "Debian Reference"
source: "https://www.debian.org/doc/manuals/debian-reference/index.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-reference/index"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-reference
aliases:
  - "Debian Reference"
---
# Debian Reference

### Osamu Aoki

Copyright Â© 2013-2024 Osamu Aoki

This Debian Reference (version 2.143) (2026-07-10 14:59:15 UTC) is intended to provide a broad overview of the Debian system as a post-installation user's guide. It covers many aspects of system administration through shell-command examples for non-developers.

Abstract

This book is free; you may redistribute it and/or modify it under the terms of the GNU General Public License of any version compliant to the Debian Free Software Guidelines (DFSG).

Table of Contents

[Preface](pr01.en.md)

[1. Disclaimer](pr01.en.md#_disclaimer)

[2. What is Debian](pr01.en.md#_what_is_debian)

[3. About this document](pr01.en.md#_about_this_document)

[3.1. Guiding rules](pr01.en.md#_guiding_rules)

[3.2. Prerequisites](pr01.en.md#_prerequisites)

[3.3. Conventions](pr01.en.md#_conventions)

[3.4. The popcon](pr01.en.md#_the_popcon)

[3.5. The package size](pr01.en.md#_the_package_size)

[3.6. Bug reports on this document](pr01.en.md#_bug_reports_on_this_document)

[4. Reminders for new users](pr01.en.md#_reminders_for_new_users)

[5. Some quotes for new users](pr01.en.md#_some_quotes_for_new_users)

[1. GNU/Linux tutorials](ch01.en.md)

[1.1. Console basics](ch01.en.md#_console_basics)

[1.1.1. The shell prompt](ch01.en.md#_the_shell_prompt)

[1.1.2. The shell prompt under GUI](ch01.en.md#_the_shell_prompt_under_gui)

[1.1.3. The root account](ch01.en.md#_the_root_account)

[1.1.4. The root shell prompt](ch01.en.md#_the_root_shell_prompt)

[1.1.5. GUI system administration tools](ch01.en.md#_gui_system_administration_tools)

[1.1.6. Virtual consoles](ch01.en.md#_virtual_consoles)

[1.1.7. How to leave the command prompt](ch01.en.md#_how_to_leave_the_command_prompt)

[1.1.8. How to shutdown the system](ch01.en.md#_how_to_shutdown_the_system)

[1.1.9. Recovering a sane console](ch01.en.md#_recovering_a_sane_console)

[1.1.10. Additional package suggestions for the newbie](ch01.en.md#_additional_package_suggestions_for_the_newbie)

[1.1.11. An extra user account](ch01.en.md#_an_extra_user_account)

[1.1.12. sudo configuration](ch01.en.md#_sudo_configuration)

[1.1.13. Play time](ch01.en.md#_play_time)

[1.2. Unix-like filesystem](ch01.en.md#_unix_like_filesystem)

[1.2.1. Unix file basics](ch01.en.md#_unix_file_basics)

[1.2.2. Filesystem internals](ch01.en.md#_filesystem_internals)

[1.2.3. Filesystem permissions](ch01.en.md#_filesystem_permissions)

[1.2.4. Control of permissions for newly created files: umask](ch01.en.md#_control_of_permissions_for_newly_created_files_umask)

[1.2.5. Permissions for groups of users (group)](ch01.en.md#_permissions_for_groups_of_users_group)

[1.2.6. Timestamps](ch01.en.md#_timestamps)

[1.2.7. Links](ch01.en.md#_links)

[1.2.8. Named pipes (FIFOs)](ch01.en.md#_named_pipes_fifos)

[1.2.9. Sockets](ch01.en.md#_sockets)

[1.2.10. Device files](ch01.en.md#_device_files)

[1.2.11. Special device files](ch01.en.md#_special_device_files)

[1.2.12. procfs and sysfs](ch01.en.md#_procfs_and_sysfs)

[1.2.13. tmpfs](ch01.en.md#_tmpfs)

[1.3. Midnight Commander (MC)](ch01.en.md#_midnight_commander_mc)

[1.3.1. Customization of MC](ch01.en.md#_customization_of_mc)

[1.3.2. Starting MC](ch01.en.md#_starting_mc)

[1.3.3. File manager in MC](ch01.en.md#_file_manager_in_mc)

[1.3.4. Command-line tricks in MC](ch01.en.md#_command_line_tricks_in_mc)

[1.3.5. The internal editor in MC](ch01.en.md#_the_internal_editor_in_mc)

[1.3.6. The internal viewer in MC](ch01.en.md#_the_internal_viewer_in_mc)

[1.3.7. Auto-start features of MC](ch01.en.md#_auto_start_features_of_mc)

[1.3.8. Virtual filesystem of MC](ch01.en.md#_virtual_filesystem_of_mc)

[1.4. The basic Unix-like work environment](ch01.en.md#_the_basic_unix_like_work_environment)

[1.4.1. The login shell](ch01.en.md#_the_login_shell)

[1.4.2. Customizing bash](ch01.en.md#_customizing_bash)

[1.4.3. Special key strokes](ch01.en.md#_special_key_strokes)

[1.4.4. Mouse operations](ch01.en.md#_mouse_operations)

[1.4.5. The pager](ch01.en.md#_the_pager)

[1.4.6. The text editor](ch01.en.md#_the_text_editor)

[1.4.7. Setting a default text editor](ch01.en.md#_setting_a_default_text_editor)

[1.4.8. Using vim](ch01.en.md#_using_vim)

[1.4.9. Recording the shell activities](ch01.en.md#_recording_the_shell_activities)

[1.4.10. Basic Unix commands](ch01.en.md#_basic_unix_commands)

[1.5. The simple shell command](ch01.en.md#_the_simple_shell_command)

[1.5.1. Command execution and environment variable](ch01.en.md#_command_execution_and_environment_variable)

[1.5.2. The "$LANG" variable](ch01.en.md#_the_literal_lang_literal_variable)

[1.5.3. The "$PATH" variable](ch01.en.md#_the_literal_path_literal_variable)

[1.5.4. The "$HOME" variable](ch01.en.md#_the_literal_home_literal_variable)

[1.5.5. Command line options](ch01.en.md#_command_line_options)

[1.5.6. Shell glob](ch01.en.md#_shell_glob)

[1.5.7. Return value of the command](ch01.en.md#_return_value_of_the_command)

[1.5.8. Typical command sequences and shell redirection](ch01.en.md#_typical_command_sequences_and_shell_redirection)

[1.5.9. Command alias](ch01.en.md#_command_alias)

[1.6. Unix-like text processing](ch01.en.md#_unix_like_text_processing)

[1.6.1. Unix text tools](ch01.en.md#_unix_text_tools)

[1.6.2. Regular expressions](ch01.en.md#_regular_expressions)

[1.6.3. Replacement expressions](ch01.en.md#_replacement_expressions)

[1.6.4. Global substitution with regular expressions](ch01.en.md#_global_substitution_with_regular_expressions)

[1.6.5. Extracting data from text file table](ch01.en.md#_extracting_data_from_text_file_table)

[1.6.6. Script snippets for piping commands](ch01.en.md#_script_snippets_for_piping_commands)

[2. Debian package management](ch02.en.md)

[2.1. Debian package management prerequisites](ch02.en.md#_debian_package_management_prerequisites)

[2.1.1. Debian package management system](ch02.en.md#_debian_package_management_system)

[2.1.2. Package configuration](ch02.en.md#_package_configuration)

[2.1.3. Basic precautions](ch02.en.md#_basic_precautions)

[2.1.4. Life with eternal upgrades](ch02.en.md#_life_with_eternal_upgrades)

[2.1.5. Debian archive basics](ch02.en.md#_debian_archive_basics)

[2.1.6. Debian is 100% free software](ch02.en.md#_debian_is_100_free_software)

[2.1.7. Package dependencies](ch02.en.md#_package_dependencies)

[2.1.8. The event flow of the package management](ch02.en.md#_the_event_flow_of_the_package_management)

[2.1.9. First response to package management troubles](ch02.en.md#_first_response_to_package_management_troubles)

[2.1.10. How to pick Debian packages](ch02.en.md#_how_to_pick_debian_packages)

[2.1.11. How to cope with conflicting requirements](ch02.en.md#_how_to_cope_with_conflicting_requirements)

[2.2. Basic package management operations](ch02.en.md#_basic_package_management_operations)

[2.2.1. apt vs. apt-get / apt-cache vs. aptitude](ch02.en.md#_literal_apt_literal_vs_literal_apt_get_literal_literal_apt_cache_literal_vs_literal_aptitude_literal)

[2.2.2. Basic package management operations with the commandline](ch02.en.md#_basic_package_management_operations_with_the_commandline)

[2.2.3. Interactive use of aptitude](ch02.en.md#_interactive_use_of_aptitude)

[2.2.4. Key bindings of aptitude](ch02.en.md#_key_bindings_of_aptitude)

[2.2.5. Package views under aptitude](ch02.en.md#_package_views_under_aptitude)

[2.2.6. Search method options with aptitude](ch02.en.md#_search_method_options_with_aptitude)

[2.2.7. The aptitude regex formula](ch02.en.md#_the_aptitude_regex_formula)

[2.2.8. Dependency resolution of aptitude](ch02.en.md#_dependency_resolution_of_aptitude)

[2.2.9. Package activity logs](ch02.en.md#_package_activity_logs)

[2.3. Examples of aptitude operations](ch02.en.md#_examples_of_aptitude_operations)

[2.3.1. Seeking interesting packages](ch02.en.md#_seeking_interesting_packages)

[2.3.2. Listing packages with regex matching on package names](ch02.en.md#_listing_packages_with_regex_matching_on_package_names)

[2.3.3. Browsing with the regex matching](ch02.en.md#_browsing_with_the_regex_matching)

[2.3.4. Purging removed packages for good](ch02.en.md#_purging_removed_packages_for_good)

[2.3.5. Tidying auto/manual install status](ch02.en.md#_tidying_auto_manual_install_status)

[2.3.6. System wide upgrade](ch02.en.md#_system_wide_upgrade)

[2.4. Advanced package management operations](ch02.en.md#_advanced_package_management_operations)

[2.4.1. Advanced package management operations with commandline](ch02.en.md#_advanced_package_management_operations_with_commandline)

[2.4.2. Verification of installed package files](ch02.en.md#_verification_of_installed_package_files)

[2.4.3. Safeguarding for package problems](ch02.en.md#_safeguarding_for_package_problems)

[2.4.4. Searching on the package meta data](ch02.en.md#_searching_on_the_package_meta_data)

[2.5. Debian package management internals](ch02.en.md#_debian_package_management_internals)

[2.5.1. Archive meta data](ch02.en.md#_archive_meta_data)

[2.5.2. Top level "Release" file and authenticity](ch02.en.md#_top_level_release_file_and_authenticity)

[2.5.3. Archive level "Release" files](ch02.en.md#_archive_level_release_files)

[2.5.4. Fetching of the meta data for the package](ch02.en.md#_fetching_of_the_meta_data_for_the_package)

[2.5.5. The package state for APT](ch02.en.md#_the_package_state_for_apt)

[2.5.6. The package state for aptitude](ch02.en.md#_the_package_state_for_aptitude)

[2.5.7. Local copies of the fetched packages](ch02.en.md#_local_copies_of_the_fetched_packages)

[2.5.8. Debian package file names](ch02.en.md#_debian_package_file_names)

[2.5.9. The dpkg command](ch02.en.md#_the_dpkg_command)

[2.5.10. The update-alternatives command](ch02.en.md#_the_update_alternatives_command)

[2.5.11. The dpkg-statoverride command](ch02.en.md#_the_dpkg_statoverride_command)

[2.5.12. The dpkg-divert command](ch02.en.md#_the_dpkg_divert_command)

[2.6. Recovery from a broken system](ch02.en.md#_recovery_from_a_broken_system)

[2.6.1. Incompatibility with old user configuration](ch02.en.md#_incompatibility_with_old_user_configuration)

[2.6.2. Caching errors of the package data](ch02.en.md#_caching_errors_of_the_package_data)

[2.6.3. Rescue with the dpkg command](ch02.en.md#_rescue_with_the_dpkg_command)

[2.6.4. Failed installation due to missing dependencies](ch02.en.md#_failed_installation_due_to_missing_dependencies)

[2.6.5. Different packages with overlapped files](ch02.en.md#_different_packages_with_overlapped_files)

[2.6.6. Fixing broken package script](ch02.en.md#_fixing_broken_package_script)

[2.6.7. Recovering package selection data](ch02.en.md#_recovering_package_selection_data)

[2.7. Tips for the package management](ch02.en.md#_tips_for_the_package_management)

[2.7.1. Who uploaded the package?](ch02.en.md#_who_uploaded_the_package)

[2.7.2. Limiting download bandwidth for APT](ch02.en.md#_limiting_download_bandwidth_for_apt)

[2.7.3. Automatic download and upgrade of packages](ch02.en.md#_automatic_download_and_upgrade_of_packages)

[2.7.4. Updates and Backports](ch02.en.md#_updates_and_backports)

[2.7.5. External package archives](ch02.en.md#_external_package_archives)

[2.7.6. Packages from mixed source of archives without apt-pinning](ch02.en.md#_packages_from_mixed_source_of_archives)

[2.7.7. Tweaking candidate version with apt-pinning](ch02.en.md#_tweaking_candidate_version)

[2.7.8. Blocking packages installed by "Recommends"](ch02.en.md#_blocking_packages_installed_by_recommends)

[2.7.9. Tracking testing with some packages from unstable](ch02.en.md#_tracking_literal_testing_literal_with_some_packages_from_literal_unstable_literal)

[2.7.10. Tracking unstable with some packages from experimental](ch02.en.md#_tracking_literal_unstable_literal_with_some_packages_from_literal_experimental_literal)

[2.7.11. Emergency downgrading](ch02.en.md#_emergency_downgrading)

[2.7.12. The equivs package](ch02.en.md#_the_equivs_package)

[2.7.13. Porting a package to the stable system](ch02.en.md#_porting_a_package_to_the_stable_system)

[2.7.14. Proxy server for APT](ch02.en.md#_proxy_server_for_apt)

[2.7.15. More readings for the package management](ch02.en.md#_more_readings_for_the_package_management)

[3. The system initialization](ch03.en.md)

[3.1. An overview of the boot strap process](ch03.en.md#_an_overview_of_the_boot_strap_process)

[3.1.1. Stage 1: the UEFI](ch03.en.md#_stage_1_the_uefi)

[3.1.2. Stage 2: the boot loader](ch03.en.md#_stage_2_the_boot_loader)

[3.1.3. Stage 3: the mini-Debian system](ch03.en.md#_stage_3_the_mini_debian_system)

[3.1.4. Stage 4: the normal Debian system](ch03.en.md#_stage_4_the_normal_debian_system)

[3.2. Rescue system](ch03.en.md#_rescue_system)

[3.2.1. GRUB UEFI rescue system on USB](ch03.en.md#_grub_uefi_rescue_system_on_usb)

[3.2.2. Linux live rescue system on USB](ch03.en.md#_linux_live_rescue_system_on_usb)

[3.2.3. Linux live rescue system from GRUB](ch03.en.md#_linux_live_rescue_system_from_grub)

[3.3. Systemd](ch03.en.md#_systemd)

[3.3.1. Systemd init](ch03.en.md#_systemd_init)

[3.3.2. Systemd login](ch03.en.md#_systemd_login)

[3.4. The kernel message](ch03.en.md#_the_kernel_message)

[3.5. The system message](ch03.en.md#_the_system_message)

[3.6. System management](ch03.en.md#_system_management_operations)

[3.7. Other system monitors](ch03.en.md#_other_system_monitors)

[3.8. System configuration](ch03.en.md#_system_configuration)

[3.8.1. The hostname](ch03.en.md#_the_hostname)

[3.8.2. The filesystem](ch03.en.md#_the_filesystem)

[3.8.3. Network interface initialization](ch03.en.md#_network_interface_initialization)

[3.8.4. Cloud system initialization](ch03.en.md#_cloud_system_initialization)

[3.8.5. Customization example to tweak sshd service](ch03.en.md#_customization_example_to_tweak_sshd_service)

[3.9. The udev system](ch03.en.md#_the_udev_system)

[3.10. The kernel module initialization](ch03.en.md#_the_kernel_module_initialization)

[4. Authentication and access controls](ch04.en.md)

[4.1. Normal Unix authentication](ch04.en.md#_normal_unix_authentication)

[4.2. Managing account and password information](ch04.en.md#_managing_account_and_password_information)

[4.3. Good password](ch04.en.md#_good_password)

[4.4. Creating encrypted password](ch04.en.md#_creating_encrypted_password)

[4.5. PAM and NSS](ch04.en.md#_pam_and_nss)

[4.5.1. Configuration files accessed by PAM and NSS](ch04.en.md#_configuration_files_accessed_by_pam_and_nss)

[4.5.2. The modern centralized system management](ch04.en.md#_the_modern_centralized_system_management)

[4.5.3. "Why GNU su does not support the wheel group"](ch04.en.md#_why_gnu_su_does_not_support_the_wheel_group)

[4.5.4. Stricter password rule](ch04.en.md#_stricter_password_rule)

[4.6. Security of authentication](ch04.en.md#_security_of_authentication)

[4.6.1. Secure password on the Internet](ch04.en.md#_secure_password_on_the_internet)

[4.6.2. Secure Shell](ch04.en.md#_secure_shell)

[4.6.3. Extra security measures for the Internet](ch04.en.md#_extra_security_measures_for_the_internet)

[4.6.4. Securing the root password](ch04.en.md#_securing_the_root_password)

[4.7. Other access controls](ch04.en.md#_other_access_controls)

[4.7.1. Access control lists (ACLs)](ch04.en.md#_access_control_lists)

[4.7.2. sudo](ch04.en.md#_sudo)

[4.7.3. PolicyKit](ch04.en.md#_policykit)

[4.7.4. Restricting access to some server services](ch04.en.md#_restricting_access_to_some_server_services)

[4.7.5. Linux security features](ch04.en.md#_linux_security_features)

[5. Network setup](ch05.en.md)

[5.1. The basic network infrastructure](ch05.en.md#_the_basic_network_infrastructure)

[5.1.1. The hostname resolution](ch05.en.md#_the_hostname_resolution)

[5.1.2. The network interface name](ch05.en.md#_the_network_interface_name)

[5.1.3. The network address range for the LAN](ch05.en.md#_the_network_address_range_for_the_lan)

[5.1.4. The network device support](ch05.en.md#_the_network_device_support)

[5.2. The modern network configuration for desktop](ch05.en.md#_the_modern_network_configuration_for_desktop)

[5.2.1. GUI network configuration tools](ch05.en.md#_gui_network_configuration_tools)

[5.3. The modern network configuration without GUI](ch05.en.md#_the_modern_network_configuration_without_gui)

[5.4. The modern network configuration for cloud](ch05.en.md#_the_modern_network_configuration_for_cloud)

[5.4.1. The modern network configuration for cloud with DHCP](ch05.en.md#_the_modern_network_configuration_for_cloud_with_dhcp)

[5.4.2. The modern network configuration for cloud with static IP](ch05.en.md#_the_modern_network_configuration_for_cloud_with_static_ip)

[5.4.3. The modern network configuration for cloud with Network Manager](ch05.en.md#_the_modern_network_configuration_for_cloud_with_network_manager)

[5.5. The low level network configuration](ch05.en.md#_the_low_level_network_configuration)

[5.5.1. Iproute2 commands](ch05.en.md#_iproute2_commands)

[5.5.2. Safe low level network operations](ch05.en.md#_safe_low_level_network_operations)

[5.6. Network optimization](ch05.en.md#_network_optimization)

[5.6.1. Finding optimal MTU](ch05.en.md#_finding_optimal_mtu)

[5.6.2. WAN TCP optimization](ch05.en.md#_wan_tcp_optimization)

[5.7. Netfilter infrastructure](ch05.en.md#_netfilter_infrastructure)

[6. Network applications](ch06.en.md)

[6.1. Web browsers](ch06.en.md#_web_browsers)

[6.1.1. Spoofing the User-Agent string](ch06.en.md#_spoofing_the_user_agent_string)

[6.1.2. Browser extension](ch06.en.md#_browser_extension)

[6.2. The mail system](ch06.en.md#_the_mail_system)

[6.2.1. Email basics](ch06.en.md#_email_basics)

[6.2.2. Modern mail service limitation](ch06.en.md#_modern_mail_service_limitation)

[6.2.3. Historic mail service expectation](ch06.en.md#_historic_mail_service_expectation)

[6.2.4. Mail transport agent (MTA)](ch06.en.md#_mail_transport_agent_mta)

[6.2.4.1. The configuration of exim4](ch06.en.md#_the_configuration_of_exim4)

[6.2.4.2. The configuration of postfix with SASL](ch06.en.md#_the_configuration_of_postfix_with_sasl)

[6.2.4.3. The mail address configuration](ch06.en.md#_the_mail_address_configuration)

[6.2.4.4. Basic MTA operations](ch06.en.md#_basic_mta_operations)

[6.3. The remote access server and utilities (SSH)](ch06.en.md#_the_remote_access_server_and_utilities_ssh)

[6.3.1. Basics of SSH](ch06.en.md#_basics_of_ssh)

[6.3.2. User name on the remote host](ch06.en.md#_user_name_on_the_remote_host)

[6.3.3. Connecting without remote passwords](ch06.en.md#_connecting_without_remote_passwords)

[6.3.4. Dealing with alien SSH clients](ch06.en.md#_dealing_with_alien_ssh_clients)

[6.3.5. Setting up ssh-agent](ch06.en.md#_setting_up_ssh_agent)

[6.3.6. Sending a mail from a remote host](ch06.en.md#_sending_a_mail_from_a_remote_host)

[6.3.7. Port forwarding for SMTP/POP3 tunneling](ch06.en.md#_port_forwarding_for_smtp_pop3_tunneling)

[6.3.8. How to shutdown the remote system on SSH](ch06.en.md#_how_to_shutdown_the_remote_system_on_ssh)

[6.3.9. Troubleshooting SSH](ch06.en.md#_troubleshooting_ssh)

[6.4. The print server and utilities](ch06.en.md#_the_print_server_and_utilities)

[6.5. Other network application servers](ch06.en.md#_other_network_application_servers)

[6.6. Other network application clients](ch06.en.md#_other_network_application_clients)

[6.7. The diagnosis of the system daemons](ch06.en.md#_the_diagnosis_of_the_system_daemons)

[7. GUI System](ch07.en.md)

[7.1. GUI desktop environment](ch07.en.md#_gui_desktops)

[7.2. GUI communication protocol](ch07.en.md#_gui_communication_protocol)

[7.3. GUI infrastructure](ch07.en.md#_gui_infrastructure)

[7.4. GUI applications](ch07.en.md#_gui_applications)

[7.5. User directories](ch07.en.md#_user_directories)

[7.6. Fonts](ch07.en.md#_fonts)

[7.6.1. Basic fonts](ch07.en.md#_basic_fonts)

[7.6.2. Font rasterization](ch07.en.md#_font_rasterization)

[7.7. Sandbox](ch07.en.md#_sandbox)

[7.8. Remote desktop](ch07.en.md#_remote_desktop)

[7.9. X server connection](ch07.en.md#_x_server_connection)

[7.9.1. X server local connection](ch07.en.md#_x_server_local_connection)

[7.9.2. X server remote connection](ch07.en.md#_x_server_remote_connection)

[7.9.3. X server chroot connection](ch07.en.md#_x_server_chroot_connection)

[7.10. Clipboard](ch07.en.md#_clipboard)

[8. I18N and L10N](ch08.en.md)

[8.1. The locale](ch08.en.md#_the_locale)

[8.1.1. Rationale for UTF-8 locale](ch08.en.md#_rationale_for_utf_8_locale)

[8.1.2. The reconfiguration of the locale](ch08.en.md#_the_reconfiguration_of_the_locale)

[8.1.3. Filename encoding](ch08.en.md#_filename_encoding)

[8.1.4. Localized messages and translated documentation](ch08.en.md#_localized_messages_and_translated_documentation)

[8.1.5. Effects of the locale](ch08.en.md#_effects_of_the_locale)

[8.2. The keyboard input](ch08.en.md#_the_keyboard_input)

[8.2.1. The keyboard input for Linux console and X Window](ch08.en.md#_the_keyboard_input_for_linux_console_and_x_window)

[8.2.2. The keyboard input for Wayland](ch08.en.md#_the_keyboard_input_for_wayland)

[8.2.3. The input method support with IBus](ch08.en.md#_the_input_method_support_with_ibus)

[8.2.4. The input method support with Fcitx](ch08.en.md#_the_input_method_support_with_fcitx)

[8.2.5. An example for Japanese](ch08.en.md#_an_example_for_japanese)

[8.3. The display output](ch08.en.md#_the_display_output)

[8.3.1. The terminal configuration](ch08.en.md#_the_terminal_configuration)

[8.3.2. East Asian Ambiguous Character Width Characters](ch08.en.md#_east_asian_ambiguous_character_width_characters)

[9. System tips](ch09.en.md)

[9.1. The console tips](ch09.en.md#_the_console_tips)

[9.1.1. Recording the shell activities cleanly](ch09.en.md#_recording_the_shell_activities_cleanly)

[9.1.2. The screen program](ch09.en.md#_the_screen_program)

[9.1.3. Navigating around directories](ch09.en.md#_navigating_around_directories)

[9.1.4. Readline wrapper](ch09.en.md#_readline_wrapper)

[9.1.5. Scanning the source code tree](ch09.en.md#_scanning_the_source_code_tree)

[9.2. Customizing vim](ch09.en.md#_customizing_vim)

[9.2.1. Customizing vim with internal features](ch09.en.md#_customizing_vim_with internal_features)

[9.2.2. Customizing vim with external packages](ch09.en.md#_customizing_vim_with_external_packages)

[9.3. Data recording and presentation](ch09.en.md#_data_recording_and_presentation)

[9.3.1. The log daemon](ch09.en.md#_the_log_daemon)

[9.3.2. Log analyzer](ch09.en.md#_log_analyzer)

[9.3.3. Customized display of text data](ch09.en.md#_customized_display_of_text_data)

[9.3.4. Customized display of time and date](ch09.en.md#_customized_display_of_time_and_date)

[9.3.5. Colorized shell echo](ch09.en.md#_colorized_shell_echo)

[9.3.6. Colorized commands](ch09.en.md#_colorized_commands)

[9.3.7. Recording the editor activities for complex repeats](ch09.en.md#_recording_the_editor_activities_for_complex_repeats)

[9.3.8. Recording the graphics image of an X application](ch09.en.md#_recording_the_graphic_image_of_an_x_application)

[9.3.9. Recording changes in configuration files](ch09.en.md#_recording_changes_in_configuration_files)

[9.4. Monitoring, controlling, and starting program activities](ch09.en.md#_monitoring_controlling_and_starting_program_activities)

[9.4.1. Timing a process](ch09.en.md#_timing_a_process)

[9.4.2. The scheduling priority](ch09.en.md#_the_scheduling_priority)

[9.4.3. The ps command](ch09.en.md#_the_ps_command)

[9.4.4. The top command](ch09.en.md#_the_top_command)

[9.4.5. Listing files opened by a process](ch09.en.md#_listing_files_opened_by_a_process)

[9.4.6. Tracing program activities](ch09.en.md#_tracing_program_activities)

[9.4.7. Identification of processes using files or sockets](ch09.en.md#_identification_of_processes_using_files_or_sockets)

[9.4.8. Repeating a command with a constant interval](ch09.en.md#_repeating_a_command_with_a_constant_interval)

[9.4.9. Repeating a command looping over files](ch09.en.md#_repeating_a_command_looping_over_files)

[9.4.10. Starting a program from GUI](ch09.en.md#_starting_a_program_from_gui)

[9.4.11. Customizing program to be started](ch09.en.md#_customizing_program_to_be_started)

[9.4.12. Killing a process](ch09.en.md#_killing_a_process)

[9.4.13. Scheduling tasks once](ch09.en.md#_scheduling_tasks_once)

[9.4.14. Scheduling tasks regularly](ch09.en.md#_scheduling_tasks_regularly)

[9.4.15. Scheduling tasks on event](ch09.en.md#_scheduling_tasks_on_event)

[9.4.16. Alt-SysRq key](ch09.en.md#_alt_sysrq_key)

[9.5. System maintenance tips](ch09.en.md#_system_maintenance_tips)

[9.5.1. Who is on the system?](ch09.en.md#_who_is_on_the_system)

[9.5.2. Warning everyone](ch09.en.md#_warning_everyone)

[9.5.3. Hardware identification](ch09.en.md#_hardware_identification)

[9.5.4. Hardware configuration](ch09.en.md#_hardware_configuration)

[9.5.5. System and hardware time](ch09.en.md#_system_and_hardware_time)

[9.5.6. The sound infrastructure](ch09.en.md#_the_sound_infrastructure)

[9.5.7. Disabling the screen saver](ch09.en.md#_disabling_the_screen_saver)

[9.5.8. Disabling beep sounds](ch09.en.md#_disabling_beep_sounds)

[9.5.9. Memory usage](ch09.en.md#_memory_usage)

[9.5.10. System security and integrity check](ch09.en.md#_system_security_and_integrity_check)

[9.6. Data storage tips](ch09.en.md#_data_storage_tips)

[9.6.1. Disk space usage](ch09.en.md#_disk_space_usage)

[9.6.2. Disk partition configuration](ch09.en.md#_disk_partition_configuration)

[9.6.3. Accessing partition using UUID](ch09.en.md#_accessing_partition_using_uuid)

[9.6.4. LVM2](ch09.en.md#_lvm2)

[9.6.5. Filesystem configuration](ch09.en.md#_filesystem_configuration)

[9.6.6. Filesystem creation and integrity check](ch09.en.md#_filesystem_creation_and_integrity_check)

[9.6.7. Optimization of filesystem by mount options](ch09.en.md#_optimization_of_filesystem_by_mount_options)

[9.6.8. Optimization of filesystem via superblock](ch09.en.md#_optimization_of_filesystem_via_superblock)

[9.6.9. Optimization of hard disk](ch09.en.md#_optimization_of_hard_disk)

[9.6.10. Optimization of solid state drive](ch09.en.md#_optimization_of_solid_state_drive)

[9.6.11. Using SMART to predict hard disk failure](ch09.en.md#_using_smart_to_predict_hard_disk_failure)

[9.6.12. Specify temporary storage directory via $TMPDIR](ch09.en.md#_specify_temporary_storage_directory_via_tmpdir)

[9.6.13. Expansion of usable storage space via LVM](ch09.en.md#_expansion_of_usable_storage_space_via_lvm)

[9.6.14. Expansion of usable storage space by mounting another partition](ch09.en.md#_expansion_of_usable_storage_space_by_mounting_another_partition)

[9.6.15. Expansion of usable storage space by bind-mounting another directory](ch09.en.md#_expansion_of_usable_storage_space_by_bind_mounting_another_directory)

[9.6.16. Expansion of usable storage space by overlay-mounting another directory](ch09.en.md#_expansion_of_usable_storage_space_by_overlay_mounting_another_directory)

[9.6.17. Expansion of usable storage space using symlink](ch09.en.md#_expansion_of_usable_storage_space_using_symlink)

[9.7. The disk image](ch09.en.md#_the_disk_image)

[9.7.1. Making the disk image file](ch09.en.md#_making_the_disk_image_file)

[9.7.2. Writing directly to the disk](ch09.en.md#_writing_directly_to_the_disk)

[9.7.3. Mounting the disk image file](ch09.en.md#_mounting_the_disk_image_file)

[9.7.4. Cleaning a disk image file](ch09.en.md#_cleaning_a_disk_image_file)

[9.7.5. Making the empty disk image file](ch09.en.md#_making_the_empty_disk_image_file)

[9.7.6. Making the ISO9660 image file](ch09.en.md#_making_the_iso9660_image_file)

[9.7.7. Writing directly to the CD/DVD-R/RW](ch09.en.md#_writing_directly_to_the_cd_dvd_r_rw)

[9.7.8. Mounting the ISO9660 image file](ch09.en.md#_mounting_the_iso9660_image_file)

[9.8. The binary data](ch09.en.md#_the_binary_data)

[9.8.1. Viewing and editing binary data](ch09.en.md#_viewing_and_editing_binary_data)

[9.8.2. Manipulating files without mounting disk](ch09.en.md#_manipulating_files_without_mounting_disk)

[9.8.3. Data redundancy](ch09.en.md#_data_redundancy)

[9.8.4. Data file recovery and forensic analysis](ch09.en.md#_data_file_recovery_and_forensic_analysis)

[9.8.5. Splitting a large file into small files](ch09.en.md#_splitting_a_large_file_into_small_files)

[9.8.6. Clearing file contents](ch09.en.md#_clearing_file_contents)

[9.8.7. Dummy files](ch09.en.md#_dummy_files)

[9.8.8. Erasing an entire hard disk](ch09.en.md#_erasing_an_entire_hard_disk)

[9.8.9. Erasing unused area of an hard disk](ch09.en.md#_erasing_unused_area_of_an_hard_disk)

[9.8.10. Undeleting deleted but still open files](ch09.en.md#_undeleting_deleted_but_still_open_files)

[9.8.11. Searching all hardlinks](ch09.en.md#_searching_all_hardlinks)

[9.8.12. Invisible disk space consumption](ch09.en.md#_invisible_disk_space_consumption)

[9.9. Data encryption tips](ch09.en.md#_data_encryption_tips)

[9.9.1. Removable disk encryption with dm-crypt/LUKS](ch09.en.md#_removable_disk_encryption_with_dm_crypt_luks)

[9.9.2. Mounting encrypted disk with dm-crypt/LUKS](ch09.en.md#_mounting_encrypted_disk_with_dm_crypt_luks)

[9.10. The kernel](ch09.en.md#_the_kernel)

[9.10.1. Kernel parameters](ch09.en.md#_kernel_parameters)

[9.10.2. Kernel headers](ch09.en.md#_kernel_headers)

[9.10.3. Compiling the kernel and related modules](ch09.en.md#_compiling_the_kernel_and_related_modules)

[9.10.4. Compiling the kernel source: Debian Kernel Team recommendation](ch09.en.md#_compiling_the_kernel_source_debian_kernel_team_recommendation)

[9.10.5. Hardware drivers and firmware](ch09.en.md#_hardware_drivers_and_firmware)

[9.11. Virtualized system](ch09.en.md#_virtualized_system)

[9.11.1. Virtualization and emulation tools](ch09.en.md#_virtualization_tools)

[9.11.2. Virtualization work flow](ch09.en.md#_virtualization_work_flow)

[9.11.3. Mounting the virtual disk image file](ch09.en.md#_mounting_the_virtual_disk_image_file)

[9.11.4. Chroot system](ch09.en.md#_chroot_system)

[9.11.5. Multiple desktop systems](ch09.en.md#_multiple_desktop_systems)

[10. Data management](ch10.en.md)

[10.1. Sharing, copying, and archiving](ch10.en.md#_sharing_copying_and_archiving)

[10.1.1. Archive and compression tools](ch10.en.md#_archive_and_compression_tools)

[10.1.2. Copy and synchronization tools](ch10.en.md#_copy_and_synchronization_tools)

[10.1.3. Idioms for the archive](ch10.en.md#_idioms_for_the_archive)

[10.1.4. Idioms for the copy](ch10.en.md#_idioms_for_the_copy)

[10.1.5. Idioms for the selection of files](ch10.en.md#_idioms_for_the_selection_of_files)

[10.1.6. Archive media](ch10.en.md#_archive_media)

[10.1.7. Removable storage device](ch10.en.md#_removable_storage_device)

[10.1.8. Filesystem choice for sharing data](ch10.en.md#_filesystem_choice_for_sharing_data)

[10.1.9. Sharing data via network](ch10.en.md#_sharing_data_via_network)

[10.2. Backup and recovery](ch10.en.md#_backup_and_recovery)

[10.2.1. Backup and recovery policy](ch10.en.md#_backup_and_recovery_policy)

[10.2.2. Backup utility suites](ch10.en.md#_backup_utility_suites)

[10.2.3. Backup tips](ch10.en.md#_backup_tips)

[10.2.3.1. GUI backup](ch10.en.md#_gui_backup)

[10.2.3.2. Mount event triggered backup](ch10.en.md#_mount_event_triggered_backup)

[10.2.3.3. Timer event triggered backup](ch10.en.md#_timer_event_triggered_backup)

[10.3. Data security infrastructure](ch10.en.md#_data_security_infrastructure)

[10.3.1. Key management for GnuPG](ch10.en.md#_key_management_for_gnupg)

[10.3.2. Using GnuPG on files](ch10.en.md#_using_gnupg_on_files)

[10.3.3. Using GnuPG with Mutt](ch10.en.md#_using_gnupg_with_mutt)

[10.3.4. Using GnuPG with Vim](ch10.en.md#_using_gnupg_with_vim)

[10.3.5. The MD5 sum](ch10.en.md#_the_md5_sum)

[10.3.6. Password keyring](ch10.en.md#_password_keyring)

[10.4. Source code merge tools](ch10.en.md#_source_code_merge_tools)

[10.4.1. Extracting differences for source files](ch10.en.md#_extracting_differences_for_source_files)

[10.4.2. Merging updates for source files](ch10.en.md#_merging_updates_for_source_files)

[10.4.3. Interactive merge](ch10.en.md#_interactive_merge)

[10.5. Git](ch10.en.md#_git)

[10.5.1. Configuration of Git client](ch10.en.md#_configuration_of_git_client)

[10.5.2. Basic Git commands](ch10.en.md#_basic_git_commands)

[10.5.3. Git tips](ch10.en.md#_git_tips)

[10.5.4. Git references](ch10.en.md#_git_references)

[10.5.5. Other version control systems](ch10.en.md#_other_version_control_systems)

[11. Data conversion](ch11.en.md)

[11.1. Text data conversion tools](ch11.en.md#_text_data_conversion_tools)

[11.1.1. Converting a text file with iconv](ch11.en.md#_converting_a_text_file_with_iconv)

[11.1.2. Checking file to be UTF-8 with iconv](ch11.en.md#_checking_file_to_be_utf_8_with_iconv)

[11.1.3. Converting file names with iconv](ch11.en.md#_converting_file_names_with_iconv)

[11.1.4. EOL conversion](ch11.en.md#_eol_conversion)

[11.1.5. TAB conversion](ch11.en.md#_tab_conversion)

[11.1.6. Editors with auto-conversion](ch11.en.md#_editors_with_auto_conversion)

[11.1.7. Plain text extraction](ch11.en.md#_plain_text_extraction)

[11.1.8. Highlighting and formatting plain text data](ch11.en.md#_highlighting_and_formatting_plain_text_data)

[11.2. XML data](ch11.en.md#_xml_data)

[11.2.1. Basic hints for XML](ch11.en.md#_basic_hints_for_xml)

[11.2.2. XML processing](ch11.en.md#_xml_processing)

[11.2.3. The XML data extraction](ch11.en.md#_the_xml_data_extraction)

[11.2.4. The XML data lint](ch11.en.md#_the_xml_data_lint)

[11.3. Type setting](ch11.en.md#_type_setting)

[11.3.1. roff typesetting](ch11.en.md#_roff_typesetting)

[11.3.2. TeX/LaTeX](ch11.en.md#_tex_latex)

[11.3.3. Pretty print a manual page](ch11.en.md#_pretty_print_a_manual_page)

[11.3.4. Creating a manual page](ch11.en.md#_creating_a_manual_page)

[11.4. Printable data](ch11.en.md#_printable_data)

[11.4.1. Ghostscript](ch11.en.md#_ghostscript)

[11.4.2. Merge two PS or PDF files](ch11.en.md#_merge_two_ps_or_pdf_files)

[11.4.3. Printable data utilities](ch11.en.md#_printable_data_utilities)

[11.4.4. Printing with CUPS](ch11.en.md#_printing_with_cups)

[11.5. The mail data conversion](ch11.en.md#_the_mail_data_conversion)

[11.5.1. Mail data basics](ch11.en.md#_mail_data_basics)

[11.6. Graphic data tools](ch11.en.md#_graphic_data_tools)

[11.6.1. Graphic data tools (metapackage)](ch11.en.md#_graphic_data_tools_meta)

[11.6.2. Graphic data tools (GUI)](ch11.en.md#_graphic_data_tools_gui)

[11.6.3. Graphic data tools (CLI)](ch11.en.md#_graphic_data_tools_cli)

[11.7. Miscellaneous data conversion](ch11.en.md#_miscellaneous_data_conversion)

[12. Programming](ch12.en.md)

[12.1. The shell script](ch12.en.md#_the_shell_script)

[12.1.1. POSIX shell compatibility](ch12.en.md#_posix_shell_compatibility)

[12.1.2. Shell parameters](ch12.en.md#_shell_parameters)

[12.1.3. Shell conditionals](ch12.en.md#_shell_conditionals)

[12.1.4. Shell loops](ch12.en.md#_shell_loops)

[12.1.5. Shell environment variables](ch12.en.md#_shell_environment_variables)

[12.1.6. The shell command-line processing sequence](ch12.en.md#_the_shell_command_line_processing_sequence)

[12.1.7. Utility programs for shell script](ch12.en.md#_utility_programs_for_shell_script)

[12.2. Scripting in interpreted languages](ch12.en.md#_scripting_in_interpreted_languages)

[12.2.1. Debugging interpreted language codes](ch12.en.md#_debugging_interpreter_codes)

[12.2.2. GUI program with the shell script](ch12.en.md#_gui_program_with_the_shell_script)

[12.2.3. Custom actions for GUI filer](ch12.en.md#_custom_actions_for_gui_filer)

[12.2.4. Perl short script madness](ch12.en.md#_perl_short_script_madness)

[12.3. Coding in compiled languages](ch12.en.md#_coding_in_compiled_languages)

[12.3.1. C](ch12.en.md#_c)

[12.3.2. Simple C program (gcc)](ch12.en.md#_simple_c_program_gcc)

[12.3.3. Flex â a better Lex](ch12.en.md#_flex_a_better_lex)

[12.3.4. Bison â a better Yacc](ch12.en.md#_bison_a_better_yacc)

[12.4. Static code analysis tools](ch12.en.md#_static_code_analysis_tools)

[12.5. Debug](ch12.en.md#_debug)

[12.5.1. Basic gdb execution](ch12.en.md#_basic_gdb_execution)

[12.5.2. Debugging the Debian package](ch12.en.md#_debugging_the_debian_package)

[12.5.3. Obtaining backtrace](ch12.en.md#_obtaining_backtrace)

[12.5.4. Advanced gdb commands](ch12.en.md#_advanced_gdb_commands)

[12.5.5. Check dependency on libraries](ch12.en.md#_check_dependency_on_libraries)

[12.5.6. Dynamic call tracing tools](ch12.en.md#_dynamic_call_tracing_tools)

[12.5.7. Debugging X Errors](ch12.en.md#_debugging_x_errors)

[12.5.8. Memory leak detection tools](ch12.en.md#_memory_leak_detection_tools)

[12.5.9. Disassemble binary](ch12.en.md#_disassemble_binary)

[12.6. Build tools](ch12.en.md#_build_tools)

[12.6.1. Make](ch12.en.md#_make)

[12.6.2. Autotools](ch12.en.md#_autotools)

[12.6.2.1. Compile and install a program](ch12.en.md#_compile_and_install_a_program)

[12.6.2.2. Uninstall program](ch12.en.md#_uninstall_program)

[12.6.3. Meson](ch12.en.md#_meson)

[12.7. Web](ch12.en.md#_web)

[12.8. The source code translation](ch12.en.md#_the_source_code_translation)

[12.9. Making Debian package](ch12.en.md#_making_debian_package)

[A. Appendix](apa.en.md)

[A.1. The Debian maze](apa.en.md#_the_debian_maze)

[A.2. Copyright history](apa.en.md#_copyright_history)

[A.3. Document format](apa.en.md#_document_format)

List of Tables

1.1. [List of interesting text-mode program packages](ch01.en.md#listofinterestineprogrampackages)
1.2. [List of informative documentation packages](ch01.en.md#listofinformativentationpackages)
1.3. [List of usage of key directories](ch01.en.md#listofusageofkeydirectories)
1.4. [List of the first character of "ls -l" output](ch01.en.md#listofthefirstchacteroflsloutput)
1.5. [The numeric mode for file permissions in chmod(1) commands](ch01.en.md#thenumericmodefoinchmodbcommands)
1.6. [The umask value examples](ch01.en.md#theumaskvalueexamples)
1.7. [List of notable system-provided groups for file access](ch01.en.md#listofnotablesysupsforfileaccess)
1.8. [List of notable system provided groups for particular command executions](ch01.en.md#listofnotablesysommandexecutions)
1.9. [List of types of timestamps](ch01.en.md#listoftypesoftimestamps)
1.10. [List of special device files](ch01.en.md#listofspecialdevicefiles)
1.11. [The key bindings of MC](ch01.en.md#thekeybindingsofmc)
1.12. [The reaction to the enter key in MC](ch01.en.md#thereactiontotheenterkeyinmc)
1.13. [List of shell programs](ch01.en.md#list-of-shell-programs)
1.14. [List of key bindings for bash](ch01.en.md#listofkeybindingsforbash)
1.15. [List of mouse operations and related key actions on Debian](ch01.en.md#listofmouseoperayactionsondebian)
1.16. [List of basic Vim key strokes](ch01.en.md#listofbasicvimkeystrokes)
1.17. [List of basic Unix commands](ch01.en.md#listofbasicunixcommands)
1.18. [The 3 parts of locale value](ch01.en.md#thedpartsoflocalevalue)
1.19. [List of locale recommendations](ch01.en.md#listoflocalerecommendations)
1.20. [List of "$HOME" values](ch01.en.md#listofhomevalues)
1.21. [Shell glob patterns](ch01.en.md#shellglobpatterns)
1.22. [Command exit codes](ch01.en.md#commandexitcodes)
1.23. [Shell command idioms](ch01.en.md#shellcommandidioms)
1.24. [Predefined file descriptors](ch01.en.md#predefinedfiledescriptors)
1.25. [Metacharacters for BRE and ERE](ch01.en.md#metacharactersforbreandere)
1.26. [The replacement expression](ch01.en.md#thereplacementexpression)
1.27. [List of script snippets for piping commands](ch01.en.md#listofscriptsniporpipingcommands)
2.1. [List of Debian package management tools](ch02.en.md#listofdebianpackemanagementtools)
2.2. [List of Debian archive sites](ch02.en.md#listofdebianarchivesites)
2.3. [List of Debian archive area](ch02.en.md#listofdebianarchivearea)
2.4. [The relationship between suite and codename](ch02.en.md#therelationshipbsuiteandcodename)
2.5. [List of key web site to resolving problems with a specific package](ch02.en.md#listofkeywebsiteaspecificpackage)
2.6. [Basic package management operations with the commandline using apt(8), aptitude(8) and apt-get(8) /apt-cache(8)](ch02.en.md#basicpackagemanaaptgetiaptcachei)
2.7. [Notable command options for aptitude(8)](ch02.en.md#notablecommandopionsforaptitudei)
2.8. [List of key bindings for aptitude](ch02.en.md#listofkeybindingsforaptitude)
2.9. [List of views for aptitude](ch02.en.md#listofviewsforaptitude)
2.10. [The categorization of standard package views](ch02.en.md#standard-package-views)
2.11. [List of the aptitude regex formula](ch02.en.md#listoftheaptituderegexformula)
2.12. [The log files for package activities](ch02.en.md#thelogfilesforpackageactivities)
2.13. [List of advanced package management operations](ch02.en.md#listofadvancedpagementoperations)
2.14. [The content of the Debian archive meta data](ch02.en.md#thecontentofthednarchivemetadata)
2.15. [The name structure of Debian packages](ch02.en.md#thenamestructureofdebianpackages)
2.16. [The usable characters for each component in the Debian package names](ch02.en.md#theusablecharactbianpackagenames)
2.17. [The notable files created by dpkg](ch02.en.md#thenotablefilescreatedbydpkg)
2.18. [List of notable Pin-Priority values for apt-pinning technique.](ch02.en.md#listofnotablepinpinningtechnique)
2.19. [List of the proxy tools specially for Debian archive](ch02.en.md#listoftheproxytofordebianarchive)
3.1. [List of boot loaders](ch03.en.md#listofbootloaders)
3.2. [The meaning of the menu entry of the above part of /boot/grub/grub.cfg](ch03.en.md#themeaningofthemfbootgrubgrubcfg)
3.3. [List of boot utilities for the Debian system](ch03.en.md#listofbootutilitrthedebiansystem)
3.4. [List of kernel error levels](ch03.en.md#listofkernelerrorlevels)
3.5. [List of typical journalctl command snippets](ch03.en.md#listoftypicaljoulcommandsnippets)
3.6. [List of typical systemctl command snippets](ch03.en.md#listoftypicalsyslcommandsnippets)
3.7. [List of other monitoring command snippets under systemd](ch03.en.md#listofothermonitpetsundersystemd)
4.1. [3 important configuration files for pam_unix(8)](ch04.en.md#dimportantconfigilesforpam_unixi)
4.2. [The second entry content of "/etc/passwd"](ch04.en.md#thesecondentrycontentofetcpasswd)
4.3. [List of commands to manage account information](ch04.en.md#listofcommandstocountinformation)
4.4. [List of tools to generate password](ch04.en.md#listoftoolstogeneratepassword)
4.5. [List of notable PAM and NSS systems](ch04.en.md#listofnotablepamandnsssystems)
4.6. [List of configuration files accessed by PAM and NSS](ch04.en.md#listofconfiguratessedbypamandnss)
4.7. [List of insecure and secure services and ports](ch04.en.md#listofinsecureanservicesandports)
4.8. [List of tools to provide extra security measures](ch04.en.md#listoftoolstoprosecuritymeasures)
5.1. [List of network configuration tools](ch05.en.md#listofnetworkconfigurationtools)
5.2. [List of network address ranges](ch05.en.md#listofnetworkaddressranges)
5.3. [Translation table from obsolete net-tools commands to new iproute2 commands](ch05.en.md#translationtableiprouteccommands)
5.4. [List of low level network commands](ch05.en.md#listoflowlevelnetworkcommands)
5.5. [List of network optimization tools](ch05.en.md#listofnetworkoptimizationtools)
5.6. [Basic guide lines of the optimal MTU value](ch05.en.md#basicguidelinesoeoptimalmtuvalue)
5.7. [List of firewall tools](ch05.en.md#listoffirewalltools)
6.1. [List of web browsers](ch06.en.md#listofwebbrowsers)
6.2. [List of mail user agent (MUA)](ch06.en.md#listofmailuseragentmua)
6.3. [List of basic mail transport agent related packages](ch06.en.md#listofbasicmailttrelatedpackages)
6.4. [List of important postfix manual pages](ch06.en.md#listofimportantpstfixmanualpages)
6.5. [List of mail address related configuration files](ch06.en.md#listofmailaddresnfigurationfiles)
6.6. [List of basic MTA operation](ch06.en.md#listofbasicmtaoperation)
6.7. [List of remote access server and utilities](ch06.en.md#listofremoteaccerverandutilities)
6.8. [List of SSH configuration files](ch06.en.md#listofsshconfigurationfiles)
6.9. [List of SSH client startup examples](ch06.en.md#listofsshclientstartupexamples)
6.10. [List of free SSH clients for other platforms](ch06.en.md#listoffreesshcliorotherplatforms)
6.11. [List of print servers and utilities](ch06.en.md#listofprintserversandutilities)
6.12. [List of other network application servers](ch06.en.md#listofothernetwoplicationservers)
6.13. [List of network application clients](ch06.en.md#listofnetworkapplicationclients)
6.14. [List of popular RFCs](ch06.en.md#listofpopularrfcs)
7.1. [List of desktop environment](ch07.en.md#listofdesktopenvironment)
7.2. [List of notable GUI infrastructure packages](ch07.en.md#listofnotableguitructurepackages)
7.3. [List of notable GUI applications](ch07.en.md#listofnotableguiapplications)
7.4. [List of notable TrueType and OpenType fonts](ch07.en.md#listofnotabletruandopentypefonts)
7.5. [List of notable font environment and related packages](ch07.en.md#listofnotablefondrelatedpackages)
7.6. [List of notable sandbox environment and related packages](ch07.en.md#listofnotablesandrelatedpackages)
7.7. [List of notable remote access server](ch07.en.md#listofnotableremoteaccessserver)
7.8. [List of connection methods to the X server](ch07.en.md#listofconnectionhodstothexserver)
7.9. [List of programs related to manipulating character clipboard](ch07.en.md#listofprogramsrearacterclipboard)
8.1. [List of IBus and its plugin packages](ch08.en.md#listofibusanditspluginpackages)
8.2. [List of Fcitx5 and its plugin packages](ch08.en.md#listoffcitxfanditspluginpackages)
9.1. [List of programs to support console activities](ch09.en.md#listofprogramstoonsoleactivities)
9.2. [List of key bindings for screen](ch09.en.md#listofkeybindingsforscreen)
9.3. [Information on the initialization of vim](ch09.en.md#informationonthetializationofvim)
9.4. [List of system log analyzers](ch09.en.md#listofsystemloganalyzers)
9.5. [Display examples of time and date for the "ls -l" command with the time style value](ch09.en.md#displayexamplesohetimestylevalue)
9.6. [List of graphics image manipulation tools](ch09.en.md#listofgraphicsimanipulationtools)
9.7. [List of packages which can record configuration history](ch09.en.md#listofpackageswhigurationhistory)
9.8. [List of tools for monitoring and controlling program activities](ch09.en.md#listoftoolsformorogramactivities)
9.9. [List of nice values for the scheduling priority](ch09.en.md#listofnicevalueshedulingpriority)
9.10. [List of ps command styles](ch09.en.md#listofpscommandstyles)
9.11. [List of frequently used signals for kill command](ch09.en.md#listoffrequentlylsforkillcommand)
9.12. [List of notable SAK command keys](ch09.en.md#listofnotablesakcommandkeys)
9.13. [List of hardware identification tools](ch09.en.md#listofhardwareidntificationtools)
9.14. [List of hardware configuration tools](ch09.en.md#listofhardwareconfigurationtools)
9.15. [List of sound packages](ch09.en.md#listofsoundpackages)
9.16. [List of commands for disabling the screen saver](ch09.en.md#listofcommandsfongthescreensaver)
9.17. [List of memory sizes reported](ch09.en.md#listofmemorysizesreported)
9.18. [List of tools for system security and integrity check](ch09.en.md#listoftoolsforsyndintegritycheck)
9.19. [List of disk partition management packages](ch09.en.md#listofdiskpartitnagementpackages)
9.20. [List of filesystem management packages](ch09.en.md#listoffilesystemnagementpackages)
9.21. [List of packages which view and edit binary data](ch09.en.md#listofpackageswhndeditbinarydata)
9.22. [List of packages to manipulate files without mounting disk](ch09.en.md#listofpackagestohoutmountingdisk)
9.23. [List of tools to add data redundancy to files](ch09.en.md#listoftoolstoaddedundancytofiles)
9.24. [List of packages for data file recovery and forensic analysis](ch09.en.md#listofpackagesfoforensicanalysis)
9.25. [List of data encryption utilities](ch09.en.md#listofdataencryptionutilities)
9.26. [List of key packages to be installed for the kernel recompilation on the Debian system](ch09.en.md#listofkeypackagenthedebiansystem)
9.27. [List of virtualization tools](ch09.en.md#listofvirtualizationtools)
10.1. [List of archive and compression tools](ch10.en.md#listofarchiveandcompressiontools)
10.2. [List of copy and synchronization tools](ch10.en.md#listofcopyandsynhronizationtools)
10.3. [List of filesystem choices for removable storage devices with typical usage scenarios](ch10.en.md#listoffilesystemalusagescenarios)
10.4. [List of the network service to choose with the typical usage scenario](ch10.en.md#listofthenetworkcalusagescenario)
10.5. [List of backup suite utilities](ch10.en.md#listofbackupsuiteutilities)
10.6. [List of data security infrastructure tools](ch10.en.md#listofdatasecurirastructuretools)
10.7. [List of GNU Privacy Guard commands for the key management](ch10.en.md#listofgnuprivacythekeymanagement)
10.8. [List of the meaning of the trust code](ch10.en.md#listofthemeaningofthetrustcode)
10.9. [List of GNU Privacy Guard commands on files](ch10.en.md#listofgnuprivacydcommandsonfiles)
10.10. [List of source code merge tools](ch10.en.md#listofsourcecodemergetools)
10.11. [List of git related packages and commands](ch10.en.md#listofgitrelatedkagesandcommands)
10.12. [Main Git commands](ch10.en.md#maingitcommands)
10.13. [Git tips](ch10.en.md#gittips)
10.14. [List of other version control system tools](ch10.en.md#list-of-vcs)
11.1. [List of text data conversion tools](ch11.en.md#listoftextdataconversiontools)
11.2. [List of encoding values and their usage](ch11.en.md#list-of-encoding-values)
11.3. [List of EOL styles for different platforms](ch11.en.md#listofeolstylesffferentplatforms)
11.4. [List of TAB conversion commands from bsdmainutils and coreutils packages](ch11.en.md#listoftabconversoreutilspackages)
11.5. [List of tools to extract plain text data](ch11.en.md#listoftoolstoextactplaintextdata)
11.6. [List of tools to highlight plain text data](ch11.en.md#listoftoolstohigghtplaintextdata)
11.7. [List of predefined entities for XML](ch11.en.md#listofpredefinedentitiesforxml)
11.8. [List of XML tools](ch11.en.md#listofxmltools)
11.9. [List of DSSSL tools](ch11.en.md#listofdsssltools)
11.10. [List of XML data extraction tools](ch11.en.md#listofxmldataextractiontools)
11.11. [List of XML pretty print tools](ch11.en.md#listofxmlprettyprinttools)
11.12. [List of type setting tools](ch11.en.md#listoftypesettingtools)
11.13. [List of packages to help creating the manpage](ch11.en.md#listofpackagestoeatingthemanpage)
11.14. [List of Ghostscript PostScript interpreters](ch11.en.md#listofghostscripriptinterpreters)
11.15. [List of printable data utilities](ch11.en.md#listofprintabledatautilities)
11.16. [List of packages to help mail data conversion](ch11.en.md#listofpackagestoildataconversion)
11.17. [List of graphics data tools (metapackage)](ch11.en.md#listofgraphicsdatoolsmetapackage)
11.18. [List of graphics data tools (GUI)](ch11.en.md#listofgraphicsdatatoolsgui)
11.19. [List of graphics data tools (CLI)](ch11.en.md#listofgraphicsdatatoolscli)
11.20. [List of miscellaneous data conversion tools](ch11.en.md#listofmiscellaneaconversiontools)
12.1. [List of typical bashisms](ch12.en.md#listoftypicalbashisms)
12.2. [List of shell parameters](ch12.en.md#listofshellparameters)
12.3. [List of shell parameter expansions](ch12.en.md#listofshellparameterexpansions)
12.4. [List of key shell parameter substitutions](ch12.en.md#listofkeyshellpatersubstitutions)
12.5. [List of file comparison operators in the conditional expression](ch12.en.md#listoffilecompartionalexpression)
12.6. [List of string comparison operators in the conditional expression](ch12.en.md#listofstringcomptionalexpression)
12.7. [List of packages containing small utility programs for shell scripts](ch12.en.md#listofpackagescosforshellscripts)
12.8. [List of interpreter related packages](ch12.en.md#listofinterpreterrelatedpackages)
12.9. [List of dialog programs](ch12.en.md#listofdialogprograms)
12.10. [List of compiler related packages](ch12.en.md#listofcompilerrelatedpackages)
12.11. [List of Yacc-compatible LALR parser generators](ch12.en.md#listofyacccompatparsergenerators)
12.12. [List of tools for static code analysis](ch12.en.md#listoftoolsforstaticcodeanalysis)
12.13. [List of debug packages](ch12.en.md#listofdebugpackages)
12.14. [List of advanced gdb commands](ch12.en.md#listofadvancedgdbcommands)
12.15. [List of memory leak detection tools](ch12.en.md#listofmemoryleakdetectiontools)
12.16. [List of build tool packages](ch12.en.md#listofbuildtoolpackages)
12.17. [List of make automatic variables](ch12.en.md#listofmakeautomaticvariables)
12.18. [List of make variable expansions](ch12.en.md#listofmakevariableexpansions)
12.19. [List of source code translation tools](ch12.en.md#listofsourcecodetranslationtools)