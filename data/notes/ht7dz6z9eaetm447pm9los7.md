
## 1:1 meeting

### MyChem paper

* Difference to existing APIs
  * integration
  * data object
  * performance
  * typical use cases
* Proving API performance
  * Perform load tests & collect statistics
* Proving integration
* Look for similar existing projects
  * Chemspider?
  * Pharos?
* Use case studies:
  * Problem: Chemicals don't have a single authoritative identifier.
  * Look for papers that do data integration from different chemical databases, and try to reproduce using MyChem queries.
  * Translator knowledge integration use cases.
* New features:
  * Separate search for drugs/compounds:
    * Configure endpoints:
      * /drug
      * /compound
  * CURIE based ID annotation searching:
    * CURIE providers:
      * BioLink model
      * identifiers.org

### ES8 testing & new features
* set up SSH access to su07
* build new ES8 docker image
* test mychem indexing


### Auth 4 Biothings
* research HMAC
* research authentication method for AWS API:
  * client_id, client_secret passed from headers
* PSQL storage tentative fields:
  * client_profile: username, email, ip, client_id, client_secret
  * client_requests: client_id, api, endpoint, method, count

### MyGeneset
  * Email Greene Lab