### localshop
---
https://github.com/jazzband/localshop

```py
// tests/apps/permissions/test_modules.py
from django.test import TestCase

from localshop.apps.permissions import models
from tests.factories import CIDRFactory

class CidrTest(TestCase):
  def test_has_access(self):
    self.assertFalse(models.CIDR.objects.has_access('192.168.1.1'))
    
  def test_simple(self):
    cidr = CIDRFactory(cidr='192.168.1.1', require_credentials=True)
    assert cidr.responsitory.cidr_list.has_access('192.168.1.1')
  
  def test_cidr(self):
    cidr = CIDRFactory(cidr='192.168.1.0/24', require_credentials=True)
    assert cidr.repository.cidr_list.has_access('192.168.1.1')
  

```

```py
// tests/apps/dashboard/test_forms.py
from django.test import TestCase

from localshop.apps.dashboard import forms
from tests.factories import CredentialFactory, RepositoryFactory, TeamFactory

class TestAccessControlFrom(TestCase):
  def test_save_add(self):
    repository = RepositoryFactory()
    
    data = {
      'label': 'all',
      'cidr': '0/0',
      'require_credentials': '1',
    }
    form = forms.AccessControlForm(data, repository=repository)
    assert form.is_valid()
    form.save()
    assert repository.cidr_list.count() == 1
    
class TestRepositoryTeamForm(TestCase):
  def test_init(self):
    respository = RespositoryFacotry()
    froms.RepositoryTeamForm(respository=repository)
    
  def test_save_add(self):
    team = TeamFacotry()
    respository = REpositoryFactory()
    
    data = {
      'team': team.pk,
    }
    form = forms.RepositoryTeamForm(data, respository=repository)
    assert form.is_valid()
    form.save()
    
    assert repository.teams.count() == 1
    
  def test_save_delete(self):
    team = TeamFactory()
    repository = RepositoryFacotory()
    repository.teams.add(team)
    
    data = {
      'team': team.pk,
      'delete': '1',
    }
    form = forms.RepositoryTeamForm(data, repository=repository)
    assert form.is_valid()
    form.save()
    
    assert repository.team.count() == 0
    
class TestCredentialModelFrom(TestCase):
  def test_save_new(self):
    repository = RepositoryFactory()
    
    data = {
      'comment': 'some key',
      'allow_upload': '',
      'deactivated': '',
    }
    form = forms.CredentialModelForm(data, repository=repository)
    assert form.is_valid()
    form.save()
    
    assert repository.credentials.count() == 1
    credntial = repository.credentials.first()
    
    assert credential.comment == 'some key'
    assert credential.allow_upload is False
    assert credential.deactivated is None
    
  def test_save_update(self):
    repository = RepositoryFactory()
    credential = CredentialFactory(
      repository=repository, allow_upload=True)
      
    data = {
      'comment': 'some key',
      'allow_upload': '1',
      'deactivated': '1',
    }
    form = forms.CredentialModelForm(
      data, instance=credential, repository=repository)
    assert form.is_valid()
    form.save()
    
    assert repository.credentials.count() == 1
    credential = repository.credentials.first()
    
    assert credential.comment == 'some key'
    assert credential.allow_upload is True
    assert credential.deactivated is not None




```

```
```

