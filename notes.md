### Design principles
- pagination & limit are purposeful left over future versions of this API. No resource nor value list is big enough to implement this in v1
- security is purposeful left over future versions of this API as of this first version
- validation rules follows the choreography principle (instead of orchestration) on which every single item must know how to behave
- fields like {warranties, surveys, documents} and others follow the override principle, successive resources over the process path may override them
- patch methods should use JSONPatch in future versions of this API
- error management must be yet defined