var onExportCSV = function (oEvent) {
			if (this.byId("listReport") && typeof this.byId("listReport").getTable === "function") {
				// if we have filter params lets put them in the filename to make it
				// more meaningful
				var sFilterExpresssion = sap.ui.model.odata.ODataUtils.createFilterParams(this._oBindingParams.filters);
				if (sFilterExpresssion) {
					// decode the uri encoding put on by the ODataUtils and replace non-alphanumerics
					sFilterExpresssion = decodeURIComponent(sFilterExpresssion).replace("$filter", "").replace(/\W+/g, "");
					//sFilterExpresssion = sFilterExpresssion.replace(/_/g, " ").replace(/eq/g, "=").replace(/and/g, " and ").replace(/or/g, " or ");
				}
				// remove the long text that is appended to the file name
				sFilterExpresssion = null;

				if (globalParams.selectedViewPerspective == "LOCATIONS") {
					var sFileName = oEvent.getSource().getModel("i18nLocations").getResourceBundle().getText("EXPORT_TABLE_NAME");
				} else {
					var sFileName = oEvent.getSource().getModel("i18nGoods").getResourceBundle().getText("EXPORT_TABLE_NAME");
				}

				if (sFilterExpresssion) {
					sFileName += "-" + sFilterExpresssion;
				}
				var oAnalyticalTableRowCount = this.byId("listReport").getTable()._getRowCount();
				if (oAnalyticalTableRowCount > 100) {
					this.byId("listReport").getTable().getModel().setSizeLimit(10000);
				}
				// filters were not applied to the exported data in the excel,
				// hence changing the binding parameters for the rowBinding
				if (this._oBindingParams.filters.length) {
					this.byId("listReport").getTable().getBinding().aFilters = this._oBindingParams.filters[0].aFilters;
				}
				/** The filters are appended with some application filters that are getting set 
					for the smarttable someowhere in the application,
					Hence, passing the rows settings so that additional filters are not applied.

					Used - this.byId("listReport").getTable().getBinding("rows").aSorter instead of aSorters 
					since aSorters remain empty for some reason
				**/
				var mSettings = {};
				mSettings.rows = {
					path: this.byId("listReport").getTable().getBindingInfo("rows").path,
					sorter: this.byId("listReport").getTable().getBinding("rows").aSorter,
					filters: this.byId("listReport").getTable().getBinding("rows").aFilters,
					parameters: this.byId("listReport").getTable().getBindingInfo("rows").parameters
				};
				// this._triggerUI5ClientExport.apply(this.byId("listReport"), arguments);
				this.byId("listReport").getTable().exportData(mSettings).saveFile(sFileName);
			}
		};
