SSSD 2.2.1
==========

Highlights
----------

### New features

- New options were added which allow sssd-kcm to handle bigger data. See manual pages for `max_ccaches`, `max_uid_caches` and `max_ccache_size`.
- SSSD can now automatically refresh cached user data from subdomains in IPA/AD trust.

### Notable bug fixes

- Fixed issue with SSSD hanging when connecting to non-responsive server with <ldaps://>
- SSSD is now restarted by systemd after crashes.
- Fixed refression when dyndns_update was set to True and dyndns_refresh_interval was not set or set to 0 then DNS records were not updated at all.
- Fixed issue when `default_domain_suffix` was used with `id_provider = files` and caused all results from files domain to be fully qualified.
- Fixed issue with sudo rules not being visible on OpenLDAP servers
- Fixed crash with `auth_provider = proxy` that prevented logins

Packaging Changes
-----------------

None

Documentation Changes
---------------------

A new option `dns_resolver_server_timeout` was added A new option `max_ccaches` was added A new option `max_uid_ccaches` was added A new option `max_ccache_size` was added A new option `ocsp_dgst` was added

Tickets Fixed
-------------

- [\#3919](https://github.com/SSSD/sssd/issues/3919) - sssd failover does not work on connecting to non-responsive <ldaps://server
- [\#4250](https://github.com/SSSD/sssd/issues/4250) - Conflicting default timeout values
- [\#4413](https://github.com/SSSD/sssd/issues/4413) - sssd-kcm cannot handle big tickets
- [\#4515](https://github.com/SSSD/sssd/issues/4515) - p11_child should work wit openssl1.0+
- [\#4704](https://github.com/SSSD/sssd/issues/4704) - KCM: Default to a new back end that would write to the secrets database directly
- [\#4827](https://github.com/SSSD/sssd/issues/4827) - port to pcre2
- [\#4880](https://github.com/SSSD/sssd/issues/4880) - multihost tests: ldb-tools is needed for multihost tests
- [\#4890](https://github.com/SSSD/sssd/issues/4890) - SSSD doesn't clear cache entries for IDs below min_id.
- [\#4983](https://github.com/SSSD/sssd/issues/4983) - SSSD is not refreshing cached user data for the ipa sub-domain in a IPA/AD trust
- [\#4996](https://github.com/SSSD/sssd/issues/4996) - EVP_PKEY_new_raw_private_key() was only added in OpenSSL 1.1.1
- [\#4998](https://github.com/SSSD/sssd/issues/4998) - sssd-kcm calls sssd-genconf which triggers nscd warning
- [\#5006](https://github.com/SSSD/sssd/issues/5006) - Logins fail after upgrade to 2.2.0
- [\#5009](https://github.com/SSSD/sssd/issues/5009) - Reasonable to Restart sssd on crashes?
- [\#5014](https://github.com/SSSD/sssd/issues/5014) - sudo: incorrect usn value for openldap
- [\#5015](https://github.com/SSSD/sssd/issues/5015) - dyndns_update = True is no longer not enough to get the IP address of the machine updated in IPA upon sssd.service startup
- [\#5018](https://github.com/SSSD/sssd/issues/5018) - nss_cmd_endservent resets the wrong index
- [\#5020](https://github.com/SSSD/sssd/issues/5020) - sssd config option "default_domain_suffix" should not cause the files domain entries to be qualified
- [\#4911](https://github.com/SSSD/sssd/issues/4911) - proxy provider is not working with enumerate=true when trying to fetch all groups
- [\#5012](https://github.com/SSSD/sssd/issues/5012) - Typo in systemd.m4 prevents detection of systemd.pc
- [\#4950](https://github.com/SSSD/sssd/issues/4950) - UPN negative cache does not use values from 'filter_users' config option
- [\#5002](https://github.com/SSSD/sssd/issues/5002) - p11_child::do_ocsp() function implementation is not FIPS140 compliant
- [\#5008](https://github.com/SSSD/sssd/issues/5008) - p11_child::sign_data() function implementation is not FIPS140 compliant
- [\#5024](https://github.com/SSSD/sssd/issues/5024) - permission denied on logs when running sssd as non-root user
- [\#4995](https://github.com/SSSD/sssd/issues/4995) - Non FIPS140 compliant usage of PRNG
- [\#3895](https://github.com/SSSD/sssd/issues/3895) - FAIL test-find-uid
- [\#4935](https://github.com/SSSD/sssd/issues/4935) - Problem with tests/cmocka/test_dyndns.c
- [\#4993](https://github.com/SSSD/sssd/issues/4993) - utils: sss_hmac_sha1() function implementation is not FIPS140 compliant
- [\#4995](https://github.com/SSSD/sssd/issues/4995) - Non FIPS140 compliant usage of PRNG
- [\#4996](https://github.com/SSSD/sssd/issues/4996) - EVP_PKEY_new_raw_private_key() was only added in OpenSSL 1.1.1

Detailed changelog
------------------

- Alex Rodin (1):

  - tests/cmocka/test_dyndns.c: Switching from tevent_loop_once() to tevent_loop_wait()

- Alexey Tikhonov (14):

  - util/crypto/libcrypto: changed sss_hmac_sha1()
  - util/crypto/libcrypto: changed sss_hmac_sha1()
  - util/secrets: memory leaks are fixed
  - util/crypto/nss/nss_nite: params sanitization
  - crypto/libcrypto/crypto_nite: HMAC calculation changed
  - util/find_uid.c: fixed debug message
  - util/find_uid.c: fixed race condition bug
  - util/crypto: removed erroneous declaration
  - util/crypto/sss_crypto.c: cleanup of includes
  - util/crypto: generate_csprng_buffer() changed
  - util/crypto: added sss_rand()
  - crypto/libcrypto/crypto_nite.c: memory leak fixed
  - FIPS140 compliant usage of PRNG
  - crypto/nss: some nss_ctx_init() params made const

- Jakub Hrozek (34):

  - Updating the version for the 2.2.1 release
  - TESTS: Install expect to drive password-change modifications
  - TESTS: Also add LDAP password when creating users
  - TESTS: Test changing LDAP password with extended operation and modification
  - TEST: Add a multihost test for not returning / for an empty home dir
  - MONITOR: Don't check for the nscd socket while regenerating configuration
  - SYSDB: Add sysdb_search_with_ts_attr
  - BE: search with sysdb_search_with_ts_attr
  - BE: Enable refresh for multiple domains
  - BE: Make be_refresh_ctx_init set up the periodical task, too
  - BE/LDAP: Call be_refresh_ctx_init() in the provider libraries, not in back end
  - BE: Pass in attribute to look up with instead of hardcoding SYSDB_NAME
  - BE: Change be_refresh_ctx_init to return errno and set be_ctx-&gt;refresh_ctx
  - BE/LDAP: Split out a helper function from sdap_refresh for later reuse
  - BE: Pass in filter_type when creating the refresh account request
  - BE: Send refresh requests in batches
  - BE: Extend be_ptask_create() with control when to schedule next run after success
  - BE: Schedule the refresh interval from the finish time of the last run
  - AD: Implement background refresh for AD domains
  - IPA: Implement background refresh for IPA domains
  - BE/IPA/AD/LDAP: Add inigroups refresh support
  - BE/IPA/AD/LDAP: Initialize the refresh callback from a list to reduce logic duplication
  - IPA/AD/SDAP/BE: Generate refresh callbacks with a macro
  - MAN: Amend the documentation for the background refresh
  - DP/SYSDB: Move the code to set initgrExpireTimestamp to a reusable function
  - IPA/AD/LDAP: Increase the initgrExpireTimestamp after finishing refresh request
  - MAN: Get rid of sssd-secrets reference
  - MAN: Document that it is enough to systemctl restart sssd-kcm.service lately
  - SECRETS: Use different option names from secrets and KCM for quota options
  - SECRETS: Don't limit the global number of ccaches
  - KCM: Pass confdb context to the ccache db initialization
  - KCM: Configurable quotas for the secdb ccache back end
  - TESTS: Add tests for the configurable quotas
  - Don't qualify users from files domain when default_domain_suffix is set

- Jakub Jelen (1):

  - pam_sss: Add missing colon to the PIN prompt

- Lukas Slebodnik (1):

  - PROXY: Return data in output parameter if everything is OK

- Michal Židek (2):

  - TESTS: ldb-tools and sssd-tools are required for multihost tests
  - Update the translations for the 2.2.1 release

- Niranjan M.R (1):

  - TESTS: Test kvno correctly displays vesion numbers of principals

- Pavel Březina (11):

  - ci: disable timeout
  - ci: switch to new tooling and remove 'Read trusted files' stage
  - ci: rebase pull request on the target branch
  - ci: print node on which the test is being run
  - sudo: use proper datetime for default modifyTimestamp value
  - systemd: add Restart=on-failure to sssd.service
  - man: fix description of dns_resolver_op_timeout
  - man: fix description of dns_resolver_timeout
  - failover: add dns_resolver_server_timeout option
  - failover: change default timeouts
  - config: add dns_resolver_op_timeout to option list

- Sam Morris (1):

  - build: fix detection of systemd.pc

- Samuel Cabrero (1):

  - nss: Fix command 'endservent' resetting wrong struct member

- Sumit Bose (10):

  - negcache: add fq-usernames of know domains to all UPN neg-caches
  - p11_child: prefer better digest function if card supports it
  - p11_child: fix a memory leak and other memory mangement issues
  - pam: make sure p11_child.log has the right permissions
  - ssh: make sure p11_child.log has the right permissions
  - BE: make sure child log files have the right permissions
  - utils: remove unused prototype (cert_to_ssh_key)
  - utils: move parse_cert_verify_opts() into separate file
  - p11_child: make OCSP digest configurable
  - pam: fix loop in Smartcard authentication

- Tomas Halman (9):

  - MAN: ldap_user_home_directory default missing
  - pcre: port to pcre2
  - CACHE: SSSD doesn't clear cache entries
  - LDAP: failover does not work on non-responsive ldaps
  - CONFDB: Files domain if activated without .conf
  - TESTS: adapt tests to enabled default files domain
  - BE: Introduce flag for be_ptask_create
  - BE: Convert be_ptask params to flags
  - DYNDNS: dyndns_update is not enough

- Yuri Chornoivan (1):

  - Fix minor typos in docs
