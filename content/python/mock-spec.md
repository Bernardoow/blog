Title: Mock com a configuração de spec.
Date: 2022-10-01 18:00
Category: Testing
Tags: Python, Testing, Mock
Slug: sqlachemy-primary-key-sysdate-oracle-problema
Authors: Bernardo Gomes
Status: draft
Lang: pt

```python
from dataclasses import dataclass
from numbers import Number


class MathOperation:
    @staticmethod
    def sum(n1: Number, n2: Number):
        return n1 + n2

class Calc:
    def sum(self, n1: Number, n2: Number) -> Number:
        result = MathOperation.sum(n1)
        return result
```

```python
from unittest.mock import Mock, patch

import pytest

from mock_spec_study.calc import Calc


@pytest.fixture
def calc():
    return Calc()


class TestCalc:
    @patch("mock_spec_study.calc.MathOperation")
    def test_sum_without_spec(self, mock_math_operation: Mock, calc: Calc):
        mock_math_operation.sum.return_value = 3
        assert calc.sum(1, 2) == 3
        mock_math_operation.sum.assert_called_once()

    @patch("mock_spec_study.calc.MathOperation", spec=True)
    def test_sum_spec(self, mock_math_operation: Mock, calc: Calc):
        mock_math_operation.sum.return_value = 3
        assert calc.sum(1, 2) == 3
        mock_math_operation.sum.assert_called_once()

    @patch("mock_spec_study.calc.MathOperation", autospec=True)
    def test_sum_autospec(self, mock_math_operation: Mock, calc: Calc):
        mock_math_operation.sum.return_value = 3
        assert calc.sum(1, 2) == 3
        mock_math_operation.sum.assert_called_once()
```
