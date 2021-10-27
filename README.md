# All-Database-Methods
Code contains all database methods that you need.




using System;
using System.Collections.Generic;
using System.Web;
using System.Data;
using System.Data.SqlClient;
using System.Data.OleDb;
using System.Configuration;

namespace Webservice.Classes
{
    public class Database
    {
        //NOTE: Methods usualy return data with sql query parameters.

        /// <summary>
        /// Database Connection
        /// </summary>
        /// <returns>SqlConnection</returns>
        public static string Connect()
        {
            string conn = ConfigurationManager.ConnectionStrings["dbConnection"].ConnectionString;
            return conn;
        }

        /// <summary>
        /// return DataTable
        /// </summary>
        /// <param name="query">sql query to select data from table </param>
        /// <returns>DataTable</returns>
        public DataTable DataTableFill(string query)
        {
            SqlConnection conn = new SqlConnection(Connect());
            try
            {
                SqlDataAdapter da = new SqlDataAdapter(query, conn);
                DataTable dt = new DataTable();
                da.Fill(dt);
                return dt;
            }
            catch (Exception ex)
            {
                throw ex;
            }
            finally
            {
                conn.Close();
            }


        }

        /// <summary>
        /// return Dataset
        /// </summary>
        /// <param name="query">sql query to select data from table</param>
        /// <returns></returns>
        public DataSet DataSetFill(string query)
        {
            SqlConnection conn = new SqlConnection(Connect());
            try
            {
                SqlDataAdapter da = new SqlDataAdapter(query, conn);
                DataSet ds = new DataSet();
                da.Fill(ds);
                return ds;
            }
            catch (Exception ex)
            {
                throw ex;
            }
            finally
            {
                conn.Close();
            }

        }

        /// <summary>
        /// return DataAdapter
        /// </summary>
        /// <param name="query">sql query to select data from table</param>
        /// <returns>DataAdapter</returns>
        public SqlDataAdapter DataAdapterFill(string query)
        {
            Connect();
            SqlCommand cmd = new SqlCommand(query);
            SqlDataAdapter da = new SqlDataAdapter(cmd);
            return da;
        }


        /// <summary>
        /// return DataReader
        /// </summary>
        /// <param name="query">sql query to select data from table</param>
        /// <returns>DataReader</returns>
        public SqlDataReader DataReaderFill(string query)
        {
            SqlConnection conn = new SqlConnection(Connect());
            SqlConnection.ClearPool(conn);
            conn.Open();
            try
            {
                SqlCommand cmd = new SqlCommand(query, conn);
                SqlDataReader dr = cmd.ExecuteReader();
                return dr;
            }
            catch (Exception ex)
            {
                throw ex;
            }
        }

        /// <summary>
        /// DataTable with SP
        /// </summary>
        /// <param name="SpName">stored procedure name</param>
        /// <param name="spParam">stored procedure parameters</param>
        /// <returns>DataTable</returns>
        public DataTable DataTableFillWithSp(string SpName, SqlParameter[] spParam)
        {
            SqlConnection conn = new SqlConnection(Connect());
            DataTable dt = new DataTable();
            conn.Open();
            try
            {
                SqlCommand cmd = new SqlCommand(SpName, conn);
                cmd.CommandType = CommandType.StoredProcedure;
                SqlDataAdapter da = new SqlDataAdapter(cmd);
                if (spParam.Length > 0)
                {
                    for (int i = 0; i < spParam.Length; i++)
                    {
                        cmd.Parameters.Add(spParam[i]);
                    }
                }
                da.Fill(dt);
            }
            catch (Exception ex)
            {
                throw ex;
            }
            finally
            {
                conn.Close();
            }
            return dt;
        }

        /// <summary>
        /// DataTable with SP
        /// </summary>
        /// <param name="SpName">stored procedure name</param>
        /// <returns>DataTable</returns>
        public DataTable DataTableFillWithSp(string SpName)
        {
            SqlConnection conn = new SqlConnection(Connect());
            DataTable dt = new DataTable();
            conn.Open();
            try
            {
                SqlCommand cmd = new SqlCommand(SpName, conn);
                cmd.CommandType = CommandType.StoredProcedure;
                SqlDataAdapter da = new SqlDataAdapter(cmd);
                da.Fill(dt);
            }
            catch (Exception ex)
            {
                throw ex;
            }
            finally
            {
                conn.Close();
            }
            return dt;
        }

        /// <summary>
        /// DataSet oluşturma with SP
        /// </summary>
        /// <param name="SpName">stored procedure name</param>
        /// <param name="spParam">stored procedure parameters</param>
        /// <returns></returns>
        public DataSet DataSetFillWithSp(string SpName, SqlParameter[] spParam)
        {
            SqlConnection conn = new SqlConnection(Connect());
            DataSet ds = new DataSet();
            conn.Open();
            try
            {
                SqlCommand cmd = new SqlCommand(SpName, conn);
                cmd.CommandType = CommandType.StoredProcedure;
                SqlDataAdapter da = new SqlDataAdapter(cmd);
                if (spParam.Length > 0)
                {
                    for (int i = 0; i < spParam.Length; i++)
                    {
                        cmd.Parameters.Add(spParam[i]);
                    }
                }
                da.Fill(ds);
            }
            catch (Exception ex)
            {
                throw ex;
            }
            finally
            {
                conn.Close();
            }
            return ds;
        }

        /// <summary>
        /// DataReader with SP
        /// </summary>
        /// <param name="SpName">stored procedure name</param>
        /// <param name="spParam">stored procedure parameters</param>
        /// <returns>DataReader</returns>
        public SqlDataReader DataReaderFillWithSp(string SpName, SqlParameter[] spParam)
        {
            SqlConnection conn = new SqlConnection(Connect());
            SqlDataReader dr;
            conn.Open();
            try
            {
                SqlCommand cmd = new SqlCommand(SpName, conn);
                if (spParam.Length > 0)
                {
                    for (int i = 0; i < spParam.Length; i++)
                    {
                        cmd.Parameters.Add(spParam[i]);
                    }
                }
                dr = cmd.ExecuteReader();
            }
            catch (Exception ex)
            {
                throw ex;
            }
            finally
            {
                conn.Close();
            }
            return dr;
        }

        /// <summary>
        /// DataReader with SP
        /// </summary>
        /// <param name="SpName">stored procedure name</param>
        /// <returns>DataReader</returns>
        public SqlDataReader DataReaderFillWithSp(string SpName)
        {
            SqlConnection conn = new SqlConnection(Connect());
            SqlDataReader dr;
            conn.Open();
            try
            {
                SqlCommand cmd = new SqlCommand(SpName, conn);
                dr = cmd.ExecuteReader();
            }
            catch (Exception ex)
            {
                throw ex;
            }
            //finally
            //{
            //    conn.Close();
            //}
            return dr;
        }

        /// <summary>
        /// Select Data return string
        /// </summary>
        /// <param name="query">sql query to select data from table</param>
        /// <returns>string</returns>
        public string SelectData(string query)
        {
            string SelectDataReturned = "";
            SqlConnection conn = new SqlConnection(Connect());
            conn.Open();
            try
            {
                SqlCommand cmd = new SqlCommand(query, conn);
                SqlDataReader dr = cmd.ExecuteReader();
                dr.Read();
                if (dr.HasRows)
                {
                    SelectDataReturned = dr.ToString();
                }
                dr.Dispose();
            }
            catch (Exception ex)
            {
                throw ex;
            }
            finally
            {
                conn.Close();
            }
            return SelectDataReturned;
        }

        // 
        /// <summary>
        /// Select Data  return table area by method tableArae parameter
        /// </summary>
        /// <param name="query">sql query to select data from table</param>
        /// <param name="tableArea">table column name</param>
        /// <returns>string</returns>
        public string SelectData(string query, string tableArea)
        {
            string SelectDataReturned = "";
            SqlConnection conn = new SqlConnection(Connect());
            conn.Open();
            try
            {
                SqlCommand cmd = new SqlCommand(query, conn);
                SqlDataReader dr = cmd.ExecuteReader();
                dr.Read();
                if (dr.HasRows)
                {
                    SelectDataReturned = dr[tableArea].ToString();
                }
                dr.Dispose();
            }
            catch (Exception ex)
            {
                throw ex;
            }
            finally
            {
                conn.Close();
            }
            return SelectDataReturned;
        }

        /// <summary>
        /// Insert to table with parameter
        /// </summary>
        /// <param name="query">sql query to insert data from table</param>
        /// <param name="param">ares to insert table </param>
        public void InsertData(string query, SqlParameter[] param)
        {
            SqlConnection conn = new SqlConnection(Connect());
            conn.Open();
            try
            {
                SqlCommand cmd = new SqlCommand(query, conn);
                for (int i = 0; i < param.Length; i++)
                {
                    cmd.Parameters.Add(param[i]);
                }
                cmd.ExecuteNonQuery();
                conn.Close();
            }
            catch (Exception ex)
            {
                throw ex;
            }
            finally
            {
                conn.Close();
            }
        }

        /// <summary>
        /// Update table with parameter
        /// </summary>
        /// <param name="query">sql query to insert data from table</param>
        /// <param name="param">ares to update table </param>
        public void UpdateData(string query, SqlParameter[] param)
        {
            SqlConnection conn = new SqlConnection(Connect());
            conn.Open();
            try
            {
                SqlCommand cmd = new SqlCommand(query, conn);
                for (int i = 0; i < param.Length; i++)
                {
                    cmd.Parameters.Add(param[i]);
                }
                cmd.ExecuteNonQuery();
                conn.Close();
            }
            catch (Exception ex)
            {
                throw ex;
            }
            finally
            {
                conn.Close();
            }
        }

        /// <summary>
        /// Delete data in the table
        /// </summary>
        /// <param name="query">sql query to delete data from table</param>
        public void DeleteData(string query)
        {
            SqlConnection conn = new SqlConnection(Connect());
            conn.Open();
            try
            {
                SqlCommand cmd = new SqlCommand(query, conn);
                cmd.ExecuteNonQuery();
            }
            catch (Exception ex)
            {
                throw ex;
            }
            finally
            {
                conn.Close();
            }
        }

        // 
        /// <summary>
        /// run any sql qurye and check result
        /// </summary>
        /// <param name="query">sql query to select data from table</param>
        /// <returns>int</returns>
        public int ProcessData(string query)
        {
            SqlConnection conn = new SqlConnection(Connect());
            conn.Open();
            try
            {
                SqlCommand cmd = new SqlCommand(query, conn);
                return cmd.ExecuteNonQuery();
            }
            catch (Exception ex)
            {
                throw ex;
            }
            finally
            {
                conn.Close();
            }
        }

        /// <summary>
        /// Run Stored Procedure with parameters 
        /// </summary>
        /// <param name="spName">stored procedure name</param>
        /// <param name="param">stored procedure parameters</param>
        /// <returns></returns>
        public int ProcessDataWithSp(string spName, SqlParameter[] param)
        {
            SqlConnection conn = new SqlConnection(Connect());
            conn.Open();
            try
            {
                SqlCommand cmd = new SqlCommand(spName, conn);
                cmd.CommandType = CommandType.StoredProcedure;
                for (int i = 0; i < param.Length; i++)
                {
                    cmd.Parameters.Add(param[i]);
                }
                return cmd.ExecuteNonQuery();
            }
            catch (SqlException ex)
            {
                throw ex;
            }
            finally
            {
                conn.Close();
            }
        }

        /// <summary>
        /// Run stored procedure without parametre
        /// </summary>
        /// <param name="spName">stored procedure name</param>
        /// <returns>int</returns>
        public int ProcessDataWithSp(string spName)
        {
            SqlConnection conn = new SqlConnection(Connect());
            conn.Open();
            try
            {
                SqlCommand cmd = new SqlCommand(spName, conn);
                cmd.CommandType = CommandType.StoredProcedure;
                return cmd.ExecuteNonQuery();
            }
            catch (SqlException ex)
            {
                throw ex;
            }
            finally
            {
                conn.Close();
            }
        }
    }
}
