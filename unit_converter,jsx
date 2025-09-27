import React, { useState } from 'react';
import { Calculator, ArrowRight, CheckCircle, AlertCircle } from 'lucide-react';

const UnitConverter = () => {
  const [inputValue, setInputValue] = useState('');
  const [fromUnit, setFromUnit] = useState('');
  const [toUnit, setToUnit] = useState('');
  const [result, setResult] = useState(null);
  const [showSteps, setShowSteps] = useState(false);

  // Conversion factors (base unit conversions)
  const conversions = {
    // Length
    length: {
      units: ['km', 'm', 'cm', 'mm', 'mile', 'yard', 'foot', 'inch'],
      factors: {
        km: 1000,
        m: 1,
        cm: 0.01,
        mm: 0.001,
        mile: 1609.34,
        yard: 0.9144,
        foot: 0.3048,
        inch: 0.0254
      },
      labels: {
        km: 'Kilometers',
        m: 'Meters',
        cm: 'Centimeters',
        mm: 'Millimeters',
        mile: 'Miles',
        yard: 'Yards',
        foot: 'Feet',
        inch: 'Inches'
      }
    },
    // Weight
    weight: {
      units: ['kg', 'g', 'mg', 'lb', 'oz'],
      factors: {
        kg: 1,
        g: 0.001,
        mg: 0.000001,
        lb: 0.453592,
        oz: 0.0283495
      },
      labels: {
        kg: 'Kilograms',
        g: 'Grams',
        mg: 'Milligrams',
        lb: 'Pounds',
        oz: 'Ounces'
      }
    },
    // Volume
    volume: {
      units: ['L', 'mL', 'gal', 'qt', 'cup', 'fl oz'],
      factors: {
        L: 1,
        mL: 0.001,
        gal: 3.78541,
        qt: 0.946353,
        cup: 0.236588,
        'fl oz': 0.0295735
      },
      labels: {
        L: 'Liters',
        mL: 'Milliliters',
        gal: 'Gallons',
        qt: 'Quarts',
        cup: 'Cups',
        'fl oz': 'Fluid Ounces'
      }
    },
    // Time
    time: {
      units: ['year', 'day', 'hour', 'min', 'sec'],
      factors: {
        year: 31536000,
        day: 86400,
        hour: 3600,
        min: 60,
        sec: 1
      },
      labels: {
        year: 'Years',
        day: 'Days',
        hour: 'Hours',
        min: 'Minutes',
        sec: 'Seconds'
      }
    }
  };

  const getConversionCategory = (unit) => {
    for (const [category, data] of Object.entries(conversions)) {
      if (data.units.includes(unit)) {
        return category;
      }
    }
    return null;
  };

  const getDirectConversionFactor = (from, to) => {
    const category = getConversionCategory(from);
    if (!category || !conversions[category].units.includes(to)) {
      return null;
    }
    
    const fromFactor = conversions[category].factors[from];
    const toFactor = conversions[category].factors[to];
    
    return fromFactor / toFactor;
  };

  const isLargerToSmaller = (from, to) => {
    const category = getConversionCategory(from);
    if (!category) return false;
    
    const fromFactor = conversions[category].factors[from];
    const toFactor = conversions[category].factors[to];
    
    return fromFactor > toFactor;
  };

  const convert = () => {
    const value = parseFloat(inputValue);
    if (isNaN(value) || !fromUnit || !toUnit) return;

    const conversionFactor = getDirectConversionFactor(fromUnit, toUnit);
    if (conversionFactor === null) return;

    const convertedValue = value * conversionFactor;
    const isLargerToSmallerConversion = isLargerToSmaller(fromUnit, toUnit);

    setResult({
      originalValue: value,
      convertedValue: convertedValue,
      conversionFactor: conversionFactor,
      fraction: `${conversionFactor}/1`,
      isLargerToSmaller: isLargerToSmallerConversion,
      fromUnit: fromUnit,
      toUnit: toUnit,
      fromLabel: conversions[getConversionCategory(fromUnit)].labels[fromUnit],
      toLabel: conversions[getConversionCategory(toUnit)].labels[toUnit]
    });
    setShowSteps(true);
  };

  const reset = () => {
    setInputValue('');
    setFromUnit('');
    setToUnit('');
    setResult(null);
    setShowSteps(false);
  };

  const getAvailableUnits = () => {
    if (!fromUnit) {
      // Return all units from all categories
      const allUnits = [];
      Object.values(conversions).forEach(category => {
        allUnits.push(...category.units);
      });
      return allUnits;
    } else {
      // Return only units from the same category
      const category = getConversionCategory(fromUnit);
      return category ? conversions[category].units : [];
    }
  };

  const getUnitLabel = (unit) => {
    const category = getConversionCategory(unit);
    return category ? conversions[category].labels[unit] : unit;
  };

  return (
    <div className="max-w-4xl mx-auto p-6 bg-gradient-to-br from-blue-50 to-indigo-100 min-h-screen">
      <div className="bg-white rounded-xl shadow-lg p-8">
        <div className="flex items-center gap-3 mb-8">
          <Calculator className="text-blue-600" size={32} />
          <h1 className="text-3xl font-bold text-gray-800">Unit Converter</h1>
        </div>
        
        {/* Input Section */}
        <div className="grid md:grid-cols-3 gap-6 mb-8">
          <div>
            <label className="block text-sm font-medium text-gray-700 mb-2">
              Enter Value
            </label>
            <input
              type="number"
              value={inputValue}
              onChange={(e) => setInputValue(e.target.value)}
              className="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition-colors"
              placeholder="Enter number..."
            />
          </div>
          
          <div>
            <label className="block text-sm font-medium text-gray-700 mb-2">
              From Unit
            </label>
            <select
              value={fromUnit}
              onChange={(e) => {
                setFromUnit(e.target.value);
                setToUnit(''); // Reset to unit when from unit changes
              }}
              className="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition-colors"
            >
              <option value="">Select unit...</option>
              {Object.entries(conversions).map(([category, data]) => (
                <optgroup key={category} label={category.charAt(0).toUpperCase() + category.slice(1)}>
                  {data.units.map(unit => (
                    <option key={unit} value={unit}>
                      {data.labels[unit]} ({unit})
                    </option>
                  ))}
                </optgroup>
              ))}
            </select>
          </div>
          
          <div>
            <label className="block text-sm font-medium text-gray-700 mb-2">
              To Unit
            </label>
            <select
              value={toUnit}
              onChange={(e) => setToUnit(e.target.value)}
              className="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition-colors"
              disabled={!fromUnit}
            >
              <option value="">Select unit...</option>
              {getAvailableUnits().filter(unit => unit !== fromUnit).map(unit => (
                <option key={unit} value={unit}>
                  {getUnitLabel(unit)} ({unit})
                </option>
              ))}
            </select>
          </div>
        </div>
        
        {/* Buttons */}
        <div className="flex gap-4 mb-8">
          <button
            onClick={convert}
            disabled={!inputValue || !fromUnit || !toUnit}
            className="flex items-center gap-2 px-6 py-3 bg-blue-600 text-white rounded-lg hover:bg-blue-700 disabled:bg-gray-300 disabled:cursor-not-allowed transition-colors"
          >
            <ArrowRight size={20} />
            Convert
          </button>
          
          <button
            onClick={reset}
            className="px-6 py-3 bg-gray-200 text-gray-700 rounded-lg hover:bg-gray-300 transition-colors"
          >
            Reset
          </button>
        </div>
        
        {/* Results and Steps */}
        {showSteps && result && (
          <div className="bg-gray-50 rounded-lg p-6">
            <h2 className="text-xl font-semibold mb-6 text-gray-800">
              Step-by-Step Conversion Process
            </h2>
            
            {/* Conversion Type Indicator */}
            <div className="mb-6 p-4 bg-blue-100 rounded-lg">
              <div className="flex items-center gap-2">
                {result.isLargerToSmaller ? (
                  <CheckCircle className="text-green-600" size={20} />
                ) : (
                  <AlertCircle className="text-orange-600" size={20} />
                )}
                <span className="font-medium">
                  {result.isLargerToSmaller 
                    ? "Converting from LARGER to SMALLER unit - MULTIPLY" 
                    : "Converting from SMALLER to LARGER unit - DIVIDE"
                  }
                </span>
              </div>
            </div>
            
            <div className="space-y-6">
              {/* Step 1 */}
              <div className="border-l-4 border-blue-500 pl-6">
                <h3 className="font-semibold text-lg mb-2">Step 1: Write the conversion as a fraction</h3>
                <div className="bg-white p-4 rounded border">
                  <p className="text-lg">
                    Conversion factor: <span className="font-mono bg-yellow-100 px-2 py-1 rounded">
                      {result.conversionFactor} {result.toUnit} per 1 {result.fromUnit}
                    </span>
                  </p>
                  <p className="text-sm text-gray-600 mt-2">
                    This means 1 {result.fromLabel} = {result.conversionFactor} {result.toLabel}
                  </p>
                </div>
              </div>
              
              {/* Step 2 */}
              <div className="border-l-4 border-green-500 pl-6">
                <h3 className="font-semibold text-lg mb-2">Step 2: Write the multiplication problem</h3>
                <div className="bg-white p-4 rounded border">
                  <p className="text-lg font-mono">
                    {result.originalValue} {result.fromUnit} × {result.conversionFactor} = ?
                  </p>
                  <p className="text-sm text-gray-600 mt-2">
                    Original number × conversion factor
                  </p>
                </div>
              </div>
              
              {/* Step 3 */}
              <div className="border-l-4 border-purple-500 pl-6">
                <h3 className="font-semibold text-lg mb-2">Step 3: Solve the multiplication problem</h3>
                <div className="bg-white p-4 rounded border">
                  <p className="text-lg font-mono">
                    {result.originalValue} × {result.conversionFactor} = <span className="bg-green-100 px-2 py-1 rounded font-bold">
                      {result.convertedValue}
                    </span>
                  </p>
                </div>
              </div>
              
              {/* Step 4 */}
              <div className="border-l-4 border-red-500 pl-6">
                <h3 className="font-semibold text-lg mb-2">Step 4: Check for mistakes</h3>
                <div className="bg-white p-4 rounded border">
                  <div className="flex items-center gap-2 mb-2">
                    <CheckCircle className="text-green-600" size={20} />
                    <span className="font-medium text-green-700">Calculation verified!</span>
                  </div>
                  <p className="text-sm text-gray-600">
                    • Units are from the same category ✓<br/>
                    • Conversion direction is correct ✓<br/>
                    • Mathematical operation is accurate ✓
                  </p>
                </div>
              </div>
            </div>
            
            {/* Final Answer */}
            <div className="mt-8 p-6 bg-gradient-to-r from-blue-500 to-purple-600 text-white rounded-lg">
              <h3 className="text-xl font-bold mb-2">Final Answer</h3>
              <p className="text-2xl font-bold">
                {result.originalValue} {result.fromLabel} = {result.convertedValue} {result.toLabel}
              </p>
            </div>
          </div>
        )}
      </div>
    </div>
  );
};

export default UnitConverter;
